# libran217.github.io 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Player</title>
    <style>
        body { font-family: Arial, sans-serif; background: linear-gradient(to right, #1e3c72, #2a5298); color: white; text-align: center; margin: 0; padding: 0; }
        header { background: rgba(0, 0, 0, 0.8); padding: 15px; }
        .container { margin: 20px; }
        .section { margin: 20px auto; padding: 20px; border-radius: 10px; max-width: 600px; background: rgba(255, 255, 255, 0.1); }
        input, button { padding: 10px; margin: 5px; border-radius: 5px; }
        button { background: #ff6f61; border: none; color: white; cursor: pointer; }
        ul { list-style: none; padding: 0; }
        li { padding: 10px; cursor: pointer; background: rgba(255, 255, 255, 0.2); margin: 5px 0; border-radius: 5px; }
    </style>
</head>
<body>

<header>
    <h1>Music Player</h1>
</header>

<div class="container">
    <section class="section">
        <h2>Music Player & Playlist</h2>
        <audio id="audioPlayer" controls></audio>
        <button onclick="playNextSong()">▶️ Next Song</button>
        <ul id="playlist"></ul>
    </section>

    <section class="section">
        <h2>Upload a Song</h2>
        <input type="file" id="fileUpload" accept="audio/*">
        <button onclick="uploadSong()">Upload</button>
    </section>

    <section class="section">
        <h2>Search Songs</h2>
        <input type="text" id="searchBox" placeholder="Search song...">
        <ul id="searchResults"></ul>
    </section>
</div>

<script>
    let playlist = [];
    let currentSongIndex = 0;
    const audioPlayer = document.getElementById('audioPlayer');
    
    function loadPlaylist() {
        playlist = [
            { name: "Song 1", url: "song1.mp3" },
            { name: "Song 2", url: "song2.mp3" },
            { name: "Song 3", url: "song3.mp3" }
        ];
        updatePlaylistUI();
        if (playlist.length > 0) playSong(0);
    }

    function updatePlaylistUI() {
        const playlistUI = document.getElementById('playlist');
        playlistUI.innerHTML = "";
        playlist.forEach((song, index) => {
            const li = document.createElement("li");
            li.textContent = song.name;
            li.onclick = () => playSong(index);
            playlistUI.appendChild(li);
        });
    }

    function playSong(index) {
        currentSongIndex = index;
        audioPlayer.src = playlist[index].url;
        audioPlayer.play();
    }

    function playNextSong() {
        if (playlist.length > 0) {
            currentSongIndex = (currentSongIndex + 1) % playlist.length;
            playSong(currentSongIndex);
        }
    }

    function uploadSong() {
        const file = document.getElementById('fileUpload').files[0];
        if (!file) return alert("Select a file first.");

        const url = URL.createObjectURL(file);
        playlist.push({ name: file.name, url });
        updatePlaylistUI();
        alert("Song added to playlist!");
    }

    function searchSongs() {
        const query = document.getElementById('searchBox').value.toLowerCase();
        const results = playlist.filter(song => song.name.toLowerCase().includes(query));

        const resultsUI = document.getElementById('searchResults');
        resultsUI.innerHTML = "";
        results.forEach(song => {
            const li = document.createElement("li");
            li.textContent = song.name;
            li.onclick = () => playSong(playlist.indexOf(song));
            resultsUI.appendChild(li);
        });
    }

    document.getElementById('searchBox').addEventListener('input', searchSongs);
    document.addEventListener("DOMContentLoaded", loadPlaylist);
</script>

</body>
</html>
