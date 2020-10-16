<template>
	<div id="app">
		<header>
			<h1>My Music</h1>
		</header>
		<main>
			<section class="player">
				<h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
				<div class="controls">
					<button class="prev" @click="prev">prev</button>
					<button class="play" v-if="!isPlaying" @click="play">Play</button>
					<button class="pause" v-else @click="pause">Pause</button>
					<button class="next" @click="next">next</button>
				</div>
			</section>
			<section class="playlist">
				<h3>The Playlist</h3>
					<table>
						<tr>
							<th>Song</th>
							<th>Artist</th>
							<th>Source</th>
							<th>Length</th>
						</tr>
						<tr>
							<th>
								<button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
									{{ song.title }}
								</button>
							</th>
							<th>
								<button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
									{{ song.artist }}
								</button>
							</th>
							<th>
								<p v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
									{{ song.src }}
								</p>
							</th>
							<th>
								<p v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
									{{ song.length }}
								</p>
							</th>
						</tr>
					</table>
			</section>
		</main>
	</div>
</template>

<script>
export default {
	name: 'App',
	data () {
		return {
			current: {},
			index: 0,
			isPlaying: false,
			songs: [
				{
					title: 'Song 1',
					artist: 'Johny Bravo',
					src: require('./assets/song_1.mp3'),
					length: '00:51'
				},
				{
					title: 'Song 2',
					artist: 'Homer Simpson',
					src: require('./assets/song_2.mp3'),
					length: '00:51'
				},
				{
					title: 'Song 3',
					artist: 'Luke Skywalker',
					src: require('./assets/song_3.mp3'),
					length: '00:51'
				},
				{
					title: 'Song 4',
					artist: 'Jeff Kilby',
					src: require('./assets/song_4.mp3'),
					length: '00:51'
				}
			],
			player: new Audio()
		}
	},
	methods: {
		play (song) {
			if (typeof song.src != "undefined") {
				this.current = song;

				this.player.src = this.current.src;
			}
			this.player.play();
			this.player.addEventListener('ended', function () {
				this.index++;
				if (this.index > this.songs.length - 1) {
					this.index = 0;
				}
			}.bind(this));
			this.isPlaying = true;
		},
		pause () {
			this.player.pause();
			this.isPlaying = false;
		},
		next () {
			this.index++;
			if (this.index > this.songs.length - 1) {
				this.index = 0;
			}
			this.current = this.songs[this.index];
			this.play(this.current);
		},
		prev () {
			this.index--;
			if (this.index < 0) {
				this.index = this.songs.length - 1;
			}
			this.current = this.songs[this.index];
			this.play(this.current);
		}
	},
	created () {
		this.current = this.songs[this.index];
		this.player.src = this.current.src;
	}
}
</script>

<style>
* {
	margin:0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: sans-serif;
}
header {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 15px;
	background-color: #212121;
	color: #fff;
}
main {
	width: 100%;
	max-width: 768px;
	margin: 0;
	padding: 25px;
}

.song-title {
	color: #212121;
	font-size: 32px;
	font-weight: 700;
	text-transform: uppercase;
	text-align: center;
}
.song-title span {
	font-weight: 400;
	font-style: italic;
}
.controls {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 30px 15px;
}
button {
	appearance: none;
	background: none;
	border: none;
	outline: none;
	cursor: pointer;
}
.play, .pause {
	font-size: 20px;
	font-weight: 700;
	padding: 15px 25px;
	margin: 0px 15px;
	border-radius: 8px;
	color: #fff;
	background-color: #CC2E5D;
}

.next, .prev {
	font-size: 16px;
	font-weight: 700;
	padding: 10px 20px;
	margin: 0px 15px;
	border-radius: 6px;
	color: #fff;
	background-color: #FF5858;
}
.playlist {
	padding: center;
}

.playlist h3 {
	color: #212121;
	font-size: 28px;
	font-weight: 400;
	margin-bottom: 30px;
	text-align: center;
}

.playlist .song {
	display: block;
	width: 100%;
	padding: 15px;
	font-size: 20px;
	font-weight: 700;
	cursor: pointer;
}
.playlist .song:hover {
	color: #FF5858;
}

.playlist .song.playing {
	color: #fff;
	background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>
