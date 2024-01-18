<script lang="ts">
  import Globe from 'globe.gl'

  const elem = document.getElementById('globe')
  if(!elem) throw new Error('No element `#globe` found.')

  const globe = (
    Globe()
    .globeImageUrl('//unpkg.com/three-globe/example/img/earth-dark.jpg')
    .bumpImageUrl('//unpkg.com/three-globe/example/img/earth-topology.png')
    .backgroundImageUrl('//unpkg.com/three-globe/example/img/night-sky.png')
    (elem)
  )

  fetch('//http-proxy.vastur.com?url=https://www.submarinecablemap.com/api/v3/cable/cable-geo.json')
  .then(r => r.json())
  .then(cablesGeo => {
    const cablePaths = []
    cablesGeo.features.forEach(({ geometry, properties }) => {
      geometry.coordinates.forEach((coords) => (
        cablePaths.push({ coords, properties })
      ))
    })

    globe
    .pathsData(cablePaths)
    .pathPoints('coords')
    .pathPointLat(p => p[1])
    .pathPointLng(p => p[0])
    .pathColor((path) => path.properties.color)
    .pathLabel((path) => path.properties.name)
    .pathDashLength(0.1)
    .pathDashGap(0.008)
    .pathDashAnimateTime(12000);
  })
</script>

<article id="globe"/>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
