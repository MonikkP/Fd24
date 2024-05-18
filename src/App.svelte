<script>
    import {songs} from "./musiclist.js";
    import {onMount} from "svelte";
    import {writable} from "svelte/store";

    let currentSongIndex = 0;
    let state = "play";
    let audioElem;
    let mainElem;

    const musicList = writable(loadFromLocalStorage());

    musicList.subscribe((currentMusicList) => {
        saveToLocalStorage(currentMusicList);
    });

    onMount(function () {
        setBackground();
    })

    $: currentSongIndex, setBackground();

    function setBackground() {
        if (mainElem && $musicList.length > 0) {
            mainElem.style.background = `linear-gradient(rgba(0,0,0,0.45), rgba(0,0,0,0.5)),
        url(./files/covers/${$musicList[currentSongIndex].image}) center no-repeat`;
            mainElem.style.backgroundSize = "cover";
        }
    }


    function loadFromLocalStorage() {
        const storedMusic = localStorage.getItem('musicList');
        if (storedMusic) {
            return JSON.parse(storedMusic);
        } else {
            return songs;
        }
    }

    function saveToLocalStorage(music) {
        localStorage.setItem('musicList', JSON.stringify(music));
    }

    function prev() {
        if (currentSongIndex === 0) {
            currentSongIndex = $musicList.length - 1;
        } else {
            currentSongIndex = (currentSongIndex - 1) % $musicList.length;
        }
        state = "play";
        //setBackground();
    }

    function playpause() {
        if (state === "play") {
            state = "pause";
            audioElem.pause();
        } else {
            state = "play";
            audioElem.play();
        }
    }

    function next() {
        currentSongIndex = (currentSongIndex + 1) % $musicList.length;
        state = "play";
        //setBackground();
    }

    function setSong(i) {
        currentSongIndex = i;
        state = "play";
        //setBackground();
    }

    function deleteSong(index) {
        musicList.update(songs => songs.filter((_, i) => i !== index));
        if (currentSongIndex >= index && currentSongIndex > 0) {
            currentSongIndex -= 1;
        }
    }

    function fetchAllSongs() {
        musicList.set(songs);
        currentSongIndex = 0;
        state = "play";
        //setBackground();
    }

</script>

<style>
    main {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }

    audio {
        display: none;
    }

    .player {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 380px;
        height: 430px;
        display: flex;
        flex-direction: column;
        border-radius: 20px;
        overflow: hidden;
    }

    .player .current-song {
        height: 120px;
        padding: 10px;
        display: flex;
        background: rgba(255, 255, 255, 0.8);
        z-index: 2;
    }

    .player .current-song .avatar {
        width: 100px;
        height: 100px;
        padding: 10px;
        text-align: center;
    }

    .player .current-song .avatar img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        object-fit: cover;
    }

    .player .current-song .song_controls {
        padding-left: 10px;
        flex: 1;
    }

    .player .current-song .song_controls h2 {
        margin-bottom: 15px;
        font-size: 20px;
        color: #111;
    }

    .player .current-song .song_controls .controls {
        display: flex;
        justify-content: space-between;
        padding-right: 40px;
    }

    .player .current-song .song_controls .controls button {
        outline: none;
        border: none;
        background: transparent;
        color: #111;
        font-size: 20px;
        cursor: pointer;
    }

    .player .song-list {
        height: calc(100% - 120px);
        background: rgba(255, 255, 255, 0.2);
        box-shadow: 0 8px 32px 0 rgba(32, 38, 135, 0.2);
        backdrop-filter: blur(5px);
        border: 1px solid rgba(255, 255, 255, 0.4);
        overflow-y: auto;
    }

    .player .song-list::-webkit-scrollbar {
        width: 4px;
        background: transparent;
    }

    .player .song-list::-webkit-scrollbar-thumb {
        width: 4px;
        background: #fff;
    }

    .player .song-list > div {
        display: flex;
        border-bottom: 1px solid rgba(255, 255, 255, 0.25);
        cursor: pointer;
        position: relative;
    }

    .player .song-list > div.active {
        background: rgba(255, 255, 255, 0.25);
    }

    .player .song-list > div .avatar {
        width: 50px;
        height: 50px;
        text-align: center;
        padding: 10px;
    }

    .player .song-list > div .avatar img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        object-fit: cover;
    }

    .player .song-list > div .song-details {
        padding: 10px;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }

    .player .song-list > div .song-details h2 {
        font-size: 16px;
        margin: 0 0 2px;
        color: #fff;
    }

    .player .song-list > div .song-details p {
        color: #eee;
        font-size: 15px;
        margin: 0;
    }

    .controls button {
        background: none;
        border: none;
        font-size: 1.5em;
        cursor: pointer;
        color: #333;
    }

    .controls button:hover {
        color: #666;
    }

    .delete-button {
        position: absolute;
        top: 50%;
        right: 10px;
        transform: translateY(-50%);
        width: 30px;
        height: 30px;
        border: none;
        border-radius: 50%;
        background-color: #ff4d4d;
        color: white;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .delete-button:hover {
        background-color: #ff1a1a;
    }

    #fetch-songs-button {
        position: fixed;
        top: 20px;
        right: 20px;
        width: 50px;
        height: 50px;
        border: none;
        border-radius: 50%;
        background-color: #4CAF50;
        color: white;
        font-size: 20px;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        transition: background-color 0.3s;
    }

    #fetch-songs-button:hover {
        background-color: #45a049;
    }

    .fa-refresh {
        font-size: 24px;
    }
</style>

<main bind:this={mainElem}>
    <audio src={"./files/songs/" + $musicList[currentSongIndex].song}
           bind:this={audioElem}
           autoplay="false"
    >
    </audio>
    <div class="player">
        <div class="current-song">
            <div class="avatar">
                <img src={"./files/covers/" + $musicList[currentSongIndex].image}>
            </div>
            <div class="song_controls">
                <h2>{$musicList[currentSongIndex].name}</h2>
                <div class="controls">
                    <button on:click={prev}>
                        <i class="fa fa-backward"></i>
                    </button>
                    <button on:click={playpause}>
                        {#if state === "play"}
                            <i class="fa fa-pause"></i>
                        {:else}
                            <i class="fa fa-play"></i>
                        {/if}
                    </button>
                    <button on:click={next}>
                        <i class="fa fa-forward"></i>
                    </button>
                </div>
            </div>
        </div>
        <div class="song-list">
            {#each $musicList as music, i}
                <div class="{i === currentSongIndex ? 'active' : ''}"
                     on:click="{() => setSong(i)}"
                >
                    <div class="avatar">
                        <img src={"./files/covers/" + music.image}>
                    </div>
                    <div class="song-details">
                        <h2>{music.name}</h2>
                        <p>{music.artist}</p>
                    </div>
                    <button class="delete-button" on:click="{() => deleteSong(i)}">
                        <i class="fa fa-trash"></i>
                    </button>
                </div>
            {/each}
        </div>
    </div>
    <button id="fetch-songs-button" on:click={fetchAllSongs}>
        <i class="fa fa-refresh"></i>
    </button>
</main>