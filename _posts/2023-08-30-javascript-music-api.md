---
comments: false
layout: default
title: JS Itunes API
description: API's are a primary source for obtaining data from the internet.  There is imformation in API's for almost any interest.
permalink: itunes
categories: [C7.0]
courses: { csse: {week: 13}, csp: {week: 4, categories: [2.C]}, csa: {week: 2} }
type: hacks
---
<!-- Input box and button for filter -->
<div>
  <input type="text" id="filterInput" placeholder="Enter iTunes filter">
  <button onclick="fetchData()">Search</button>
</div>

<!-- HTML table fragment for page -->
<table>
  <thead>
    <tr>
      <th>Artist</th>
      <th>Track</th>
      <th>Images</th>
      <th>Preview</th>
    </tr>
  </thead>
  <tbody id="result">
    <!-- generated rows -->
  </tbody>
</table>

<h2> Recently Searched </h2>

<table>
  <thead>
    <tr>
      <th>Artist</th>
      <th>Track</th>
      <th>Images</th>
      <th>Preview</th>
    </tr>
  </thead>
  <tbody id="local-storage">
    <!-- generated rows -->
  </tbody>
</table>

<!-- Script is laid out in a sequence (no function) and will execute when the page is loaded -->
<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");
  const localStorageContainer = document.getElementById("local-storage");

  // function to fetch data based on user input
  function fetchData() {
    // clear previous results
    resultContainer.innerHTML = "";

    // get user input
    const filterInput = document.getElementById("filterInput");
    const filter = filterInput.value;

    // prepare fetch options
    const url = "https://itunes.apple.com/search?term=" + encodeURIComponent(filter);
    const headers = {
      method: 'GET',
      mode: 'cors',
      cache: 'default',
      credentials: 'omit',
      headers: {
        'Content-Type': 'application/json'
      },
    };

    // fetch the API
    fetch(url, headers)
      .then(response => {
        // check for response errors
        if (response.status !== 200) {
          const errorMsg = 'Database response error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
        }
        // valid response will have JSON data
        response.json().then(data => {
          console.log(data);

          // Music data - limit to the first five results
          for (let i = 0; i < 5 && i < data.results.length; i++) {
            const row = data.results[i];

            // tr for each row
            const tr = document.createElement("tr");
            // td for each column
            const artist = document.createElement("td");
            const track = document.createElement("td");
            const image = document.createElement("td");
            const preview = document.createElement("td");

            // data is specific to the API
            artist.innerHTML = row.artistName;
            track.innerHTML = row.trackName;
            // create preview image
            const img = document.createElement("img");
            img.src = row.artworkUrl100;
            image.appendChild(img);
            // create preview player
            const audio = document.createElement("audio");
            audio.controls = true;
            const source = document.createElement("source");
            source.src = row.previewUrl;
            source.type = "audio/mp4";
            audio.appendChild(source);
            preview.appendChild(audio);

            // this builds td's into tr
            tr.appendChild(artist);
            tr.appendChild(track);
            tr.appendChild(image);
            tr.appendChild(preview);

            // add HTML to container
            resultContainer.appendChild(tr);

            // Store data in local storage
            const recentlySearched = JSON.parse(localStorage.getItem("recentlySearched")) || [];
            recentlySearched.push({
              artist: row.artistName,
              track: row.trackName,
              image: row.artworkUrl100,
              preview: row.previewUrl,
            });
            localStorage.setItem("recentlySearched", JSON.stringify(recentlySearched));

            // Update the Recently Searched table
            updateRecentlySearchedTable();
          }
        });
      })
      .catch(err => {
        console.error(err);
        const tr = document.createElement("tr");
        const td = document.createElement("td");
        td.innerHTML = err;
        tr.appendChild(td);
        resultContainer.appendChild(tr);
      });
  }

  // Function to update the Recently Searched table
  function updateRecentlySearchedTable() {
    const recentlySearched = JSON.parse(localStorage.getItem("recentlySearched")) || [];
    const localStorageContainer = document.getElementById("local-storage");
    localStorageContainer.innerHTML = "";

    for (const item of recentlySearched) {
      const tr = document.createElement("tr");
      const artist = document.createElement("td");
      const track = document.createElement("td");
      const image = document.createElement("td");
      const preview = document.createElement("td");

      artist.innerHTML = item.artist;
      track.innerHTML = item.track;

      // create preview image
      const img = document.createElement("img");
      img.src = item.image;
      image.appendChild(img);

      // create preview player
      const audio = document.createElement("audio");
      audio.controls = true;
      const source = document.createElement("source");
      source.src = item.preview;
      source.type = "audio/mp4";
      audio.appendChild(source);
      preview.appendChild(audio);

      tr.appendChild(artist);
      tr.appendChild(track);
      tr.appendChild(image);
      tr.appendChild(preview);

      localStorageContainer.appendChild(tr);
    }
  }

  // Call updateRecentlySearchedTable to initialize it
  updateRecentlySearchedTable();
</script>





## Hacks
The endpoint itunes.apple.com allows requests and responses on their `data`.   We provide the Input and Output interaction with the itunes data;  however, we do not create or manage the data.  There is a backend process that creates and stores data.  

In this type of Website relationship we could provide..
- A better starting screen.  Providing sample queries on screen.
-  Provide a local storage to show recent or liked queries by the user.

But, we would need backend help to..
- Show most popular queries.
- Add or modify posts.
