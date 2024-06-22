<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Video Player</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="videocontrols">
        <div class="controlsContainer">
            <div class="controlBar">
                <button class="playButton" aria-label="Play"></button>
                <button class="muteButton" aria-label="Mute"></button>
                <button class="fullscreenButton" aria-label="Fullscreen"></button>
                <div class="progressBackgroundBar">
                    <div class="progressBar"></div>
                    <div class="bufferBar"></div>
                    <div class="scrubber"></div>
                </div>
                <div class="textTrackListContainer">
                    <ul class="textTrackList">
                        <li class="textTrackItem">English</li>
                        <li class="textTrackItem">Spanish</li>
                        <li class="textTrackItem">French</li>
                    </ul>
                </div>
                <div class="positionDurationBox">
                    <span class="positionLabel">00:00</span>
                    <span class="durationLabel">/ 01:30</span>
                </div>
            </div>
            <div class="statusOverlay">
                <div class="statusIcon" type="throbber"></div>
                <div class="statusLabel">Loading...</div>
            </div>
            <div class="pictureInPictureOverlay">
                <div class="pictureInPictureToggleLabel">Picture in Picture</div>
                <button class="pictureInPictureToggle" aria-label="Toggle Picture in Picture"></button>
            </div>
        </div>
        <video id="video" width="100%" height="100%" controls>
            <source src="video.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>
    </div>

    <script>
        // Get the video and control elements
        const video = document.getElementById('video');
        const playButton = document.querySelector('.playButton');
        const muteButton = document.querySelector('.muteButton');
        const fullscreenButton = document.querySelector('.fullscreenButton');
        const scrubber = document.querySelector('.scrubber');
        const progressBar = document.querySelector('.progressBar');
        const bufferBar = document.querySelector('.bufferBar');
        const textTrackList = document.querySelector('.textTrackList');
        const pictureInPictureToggle = document.querySelector('.pictureInPictureToggle');

        // Add event listeners
        playButton.addEventListener('click', togglePlay);
        muteButton.addEventListener('click', toggleMute);
        fullscreenButton.addEventListener('click', toggleFullscreen);
        scrubber.addEventListener('input', seek);
        textTrackList.addEventListener('click', selectTextTrack);
        pictureInPictureToggle.addEventListener('click', togglePictureInPicture);

        // Initialize the video player
        video.addEventListener('loadedmetadata', initVideo);

        // Function to toggle play/pause
        function togglePlay() {
            if (video.paused) {
                video.play();
            } else {
                video.pause();
            }
        }

        // Function to toggle mute/unmute
        function toggleMute() {
            video.muted = !video.muted;
        }

        // Function to toggle fullscreen
        function toggleFullscreen() {
            if (document.fullscreenElement) {
                document.exitFullscreen();
            } else {
                video.requestFullscreen();
            }
        }

        // Function to seek to a new position
        function seek(event) {
            video.currentTime = event.target.value;
        }

        // Function to select a new text track
        function selectTextTrack(event) {
            const track = event.target.textContent;
            video.textTracks.forEach((track) => {
                if (track.label === track) {
                    track.mode = 'showing';
                } else {
                    track.mode = 'disabled';
                }
            });
        }

        // Function to toggle picture-in-picture
        function togglePictureInPicture() {
            if (document.pictureInPictureEnabled) {
                video.requestPictureInPicture();
            } else {
                document.exitPictureInPicture();
            }
        }

        // Function to initialize the video player
        function initVideo() {
            // Set the initial progress bar and buffer bar values
            progressBar.style.width = '0%';
            bufferBar.style.width = '0%';

            // Set the initial text track
            video.textTracks[0].mode = 'showing';

            // Add event listeners for video events
            video.addEventListener('timeupdate', updateProgressBar);
            video.addEventListener('progress', updateBufferBar);
        }

        // Function to update the progress bar
        function update
