---
comments: false
layout: default
title: JS Itunes API
description: API's are a primary source for obtaining data from the internet.  There is imformation in API's for almost any interest.
permalink: itunes
categories: [C7.0]
courses: { csse: {week: 13}, csp: {week: 4, categories: [2.C]}, csa: {week: 2} }
type: ccc
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
      <th>Like</th> <!-- New column for the like button -->
    </tr>
  </thead>
  <tbody id="result">
    <!-- generated rows -->
  </tbody>
</table>

<!-- Separate table to display liked songs -->
<table>
  <thead>
    <tr>
      <th>Liked Songs</th>
    </tr>
  </thead>
  <tbody id="likedTableBody">
    <!-- Liked songs will be added here -->
  </tbody>
</table>


<!-- Script is laid out in a sequence (no function) and will execute when the page is loaded -->
<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");

  // ...
// Function to display liked songs in a separate table
function displayLikedSongs() {
  const likedSongs = JSON.parse(localStorage.getItem("likedSongs")) || [];

  // Get or create a table for liked songs
  let likedTable = document.getElementById("likedTable");
  if (!likedTable) {
    likedTable = document.createElement("table");
    likedTable.id = "likedTable";
    const headerRow = document.createElement("tr");
    const headerCell = document.createElement("th");
    headerCell.textContent = "Liked Songs";
    headerRow.appendChild(headerCell);
    likedTable.appendChild(headerRow);
    document.body.appendChild(likedTable);
  }

  // Clear existing rows
  likedTable.innerHTML = "";

  // Populate the table with liked songs
  likedSongs.forEach(songName => {
    const row = document.createElement("tr");
    const cell = document.createElement("td");
    cell.textContent = songName;
    row.appendChild(cell);
    likedTable.appendChild(row);
  });
}

// Call the function to display liked songs (you can trigger this as needed)
displayLikedSongs();

// function to handle liking a song
// function to handle liking a song
function likeSong(trackName) {
  try {
    // Check if the song is already liked
    const likedSongs = JSON.parse(localStorage.getItem("likedSongs")) || [];
    if (!likedSongs.includes(trackName)) {
      likedSongs.push(trackName);
      localStorage.setItem("likedSongs", JSON.stringify(likedSongs));
      console.log(`Liked: ${trackName}`);
    }
  } catch (error) {
    console.error("Error liking song:", error);
  }
}

// function to render the "like" button in a row
function renderLikeButton(trackName) {
  try {
    const likeButton = document.createElement("button");
    likeButton.textContent = "Like";
    likeButton.addEventListener("click", () => {
      likeSong(trackName);
      alert(`You liked the song: ${trackName}`);
    });
    return likeButton;
  } catch (error) {
    console.error("Error rendering like button:", error);
    return null;
  }
}


// ...

// Inside the loop where you create rows for each song
// ...
// create "like" button
const likeButton = renderLikeButton(row.trackName);
const likeColumn = document.createElement("td");
likeColumn.appendChild(likeButton);
tr.appendChild(likeColumn);
// ...



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

        // Music data
        for (const row of data.results) {
          console.log(row);

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

          // create "like" button
          const likeButton = renderLikeButton(row.trackName);
          const likeColumn = document.createElement("td");
          likeColumn.appendChild(likeButton);

          // this builds td's into tr
          tr.appendChild(artist);
          tr.appendChild(track);
          tr.appendChild(image);
          tr.appendChild(preview);
          tr.appendChild(likeColumn); // Add the "Like" button column

          // add HTML to container
          resultContainer.appendChild(tr);
        }
      })
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
</script>

## Hacks
The endpoint itunes.apple.com allows requests and responses on their `data`.   We provide the Input and Output interaction with the itunes data;  however, we do not create or manage the data.  There is a backend process that creates and stores data.  

In this type of Website relationship we could provide..
- A better starting screen.  Providing sample queries on screen.
-  Provide a local storage to show recent or liked queries by the user.

But, we would need backend help to..
- Show most popular queries.
- Add or modify posts.
