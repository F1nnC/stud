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

<!-- Input box and button for filter -->
<div style="margin: auto;">
  <input type="text" id="filterInput" placeholder="Enter iTunes filter" style="padding: 5px; width: 250px; height: 50px; border-radius: 5px;">
  <button onclick="fetchData()" style="height: 50px;">Search</button>
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


<!-- Script is laid out in a sequence (no function) and will execute when the page is loaded -->
<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");

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

      // Update the "Like" button for liked songs
      updateLikeButton(songName, true);
    });
    console.log("hi");
  }

  // Call the function to display liked songs (you can trigger this as needed)
  displayLikedSongs();

  // Function to handle liking a song
  function likeSong(trackName) {
    try {
      // Check if the song is already liked
      const likedSongs = JSON.parse(localStorage.getItem("likedSongs")) || [];
      if (!likedSongs.includes(trackName)) {
        likedSongs.push(trackName);
        localStorage.setItem("likedSongs", JSON.stringify(likedSongs));
        console.log(`Liked: ${trackName}`);
        updateLikeButton(trackName, true); // Update the "Like" button text
      } else {
        // If the song is already liked, you can choose to implement an "unlike" feature here.
        console.log(`Already liked: ${trackName}`);
      }
    } catch (error) {
      console.error("Error liking song:", error);
    }
  }

  // Function to update the "Like" button text and behavior
  function updateLikeButton(trackName, isLiked) {
    try {
      const likeButtons = document.querySelectorAll(".like-button");
      likeButtons.forEach(button => {
        if (button.dataset.trackName === trackName) {
          if (isLiked) {
            button.textContent = "Liked";
            button.disabled = true;
          } else {
            button.textContent = "Like";
            button.disabled = false;
          }
        }
      });
    } catch (error) {
      console.error("Error updating like button:", error);
    }
  }

  // Function to render the "Like" button in a row
  function renderLikeButton(trackName) {
    try {
      const likeButton = document.createElement("button");
      likeButton.textContent = "Like";
      likeButton.classList.add("like-button"); // Add a class for easier selection
      likeButton.dataset.trackName = trackName; // Store the track name as a data attribute
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


// Function to fetch data based on user input
function fetchData() {
  // Clear previous results
  resultContainer.innerHTML = "";

  // Get user input
  const filterInput = document.getElementById("filterInput");
  const filter = filterInput.value;

  // Prepare fetch options
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

  // Fetch the API
  fetch(url, headers)
    .then(response => {
      // Check for response errors
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
      // Valid response will have JSON data
      response.json().then(data => {
        console.log(data);

        // Music data
        for (const row of data.results) {
          console.log(row);

          // Create a table row for each song
          const tr = document.createElement("tr");

          // Create table cells for artist, track, image, and preview
          const artist = document.createElement("td");
          const track = document.createElement("td");
          const image = document.createElement("td");
          const preview = document.createElement("td");

          // Populate data from the API into table cells
          artist.innerHTML = row.artistName;
          track.innerHTML = row.trackName;

          // Create an image element for the artwork
          const img = document.createElement("img");
          img.src = row.artworkUrl100;
          image.appendChild(img);

          // Create an audio player for the preview
          const audio = document.createElement("audio");
          audio.controls = true;
          const source = document.createElement("source");
          source.src = row.previewUrl;
          source.type = "audio/mp4";
          audio.appendChild(source);
          preview.appendChild(audio);

          // Create and append the "Like" button column
          const likeButton = renderLikeButton(row.trackName);
          const likeColumn = document.createElement("td");
          likeColumn.appendChild(likeButton);
          tr.appendChild(likeColumn);

          // Append all table cells to the table row
          tr.appendChild(artist);
          tr.appendChild(track);
          tr.appendChild(image);
          tr.appendChild(preview);
          tr.appendChild(likeColumn); // Add the "Like" button column

          // Add the table row to the result container
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
