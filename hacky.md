# Hacky stuff to make things work
## Browser Cache Issue on Firefox
- jira: https://intelehub.atlassian.net/browse/DN-840
- issue: after customizing the pbiviz, it doesnt seem to work on firefox(does not display features)
- cause: seems to becaused by caching object not available/setup correctly for firefox (should be due to tighter browser security)
- solution:
    - see: `node_modules/mapbox-gl/src/util/tile_request_cache.js` @ln:25 @function cacheOpen(), <br />
      if we just `return;` for the function, this will bypass the usage of browser cache completely
    - because this is a library reference, we need to meddle with the `dist` code directly (the one that gets packaged by pbiviz), 
      `node_modules/mapbox-gl/dist/mapbox-gl.js`, search for `o.caches.open("mapbox-tiles")`,
      rewind a bit till where it says `function lt(){`, add `return;` after it immediately to skip the caching