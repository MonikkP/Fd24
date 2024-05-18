# Music Player Application

This Music Player is a web application built using Svelte, showcasing modern web development practices with Svelte's
reactivity, component management, and state handling features.

## Framework Features used

- **Lifecycle Hooks**: The onMount hook specifically runs after the component has been first rendered to the DOM. The
  function passed to `onMount` will be executed once the component has been rendered in the DOM. In this case,
  `setBackground` will be called to set the background of the main element.
- **Local Storage Interaction**: When the application starts, `loadFromLocalStorage` is called to initialize
  the `musicList`
  store with either the data from local storage (if it exists) or the default list of songs. We subscribe to changes in
  the `musicList` store. Whenever the store's value changes, the `saveToLocalStorage` function is called to update the
  local
  storage.
- **Event Handling**: Includes handling of user interactions like clicks for playback controls and song selection.
- **Conditional Rendering**: Dynamically renders UI elements based on the state, such as toggling play/pause icons and
  highlighting the current song.
- **Component Bindings**: Uses `bind:this` for direct DOM manipulations, a core feature of Svelte for reactive updates.
  Example: bind mainElem to `<main>`and audioElem to `<audio>`, allowing us to manipulate these elements directly in
  script.
- **Style Management**: Demonstrates the use of external and scoped CSS for styling.
- **List Rendering and Keyed Each Blocks**: Efficiently manages lists with Svelte's `{#each}` blocks for optimized
  rendering.
- **Store for State Management**: Employs a Svelte store to manage the global state of the song list reactively across
  components. By subscribing to the `musicList` store, we set up a listener that reacts to any changes in
  the `musicList` store.
- **Reactive Programming**: Svelte automatically re-renders the UI when state variable `musicList` is changed. Using
  reactive statements like  `$: $musicList, currentSongIndex, setBackground()` we can call setBackground
  whenever `currentSongIndex` is changed.

## Application Overview

This Svelte-based music player application allows users to manage and play a list of songs. It provides functionalities
to play, pause, navigate through songs, and maintain a playlist with persistent state across sessions using local
storage.

### Key Features

1. Song Playback Controls
2. Song List Management
3. Persistent State with Local Storage
4. Fetch All Available Songs Button
5. Responsive Design
6. Dynamic Background

## About Svelte

- **Compiler-Based**: Faster - Svelte is a compiler that converts your components into highly optimized JavaScript at
  build time. This
  means there is no framework-specific runtime overhead in the browser.
- **Reactive Programming**: Svelte automatically re-renders the UI when a state variable changes.
- **No Virtual DOM**: Since Svelte updates the DOM directly with compiled JavaScript, it can often be more efficient
  than frameworks that use a virtual DOM.
- **Simplicity**: Svelte components are written in a single file with HTML, CSS, and JavaScript, making it
  straightforward to understand and work with.
- **Less Boilerplate**: Svelte's reactivity model reduces the amount of boilerplate code needed to manage state and
  props.
"# Fd24" 
