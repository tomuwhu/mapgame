<script lang="ts">
  //@ts-nocheck
   import { onMount } from "svelte";
   var ol = {}
   var selc = ''
   onMount(async () => {
      let parser = new DOMParser()
      let text:any = await fetch('worldmap.svg')
      text = await text.text()
      let xmlDoc = parser.parseFromString(text,"text/xml")
      let paths = xmlDoc.getElementsByTagName("svg")[0].children
      for (let i = 1; i < paths.length; i++) {
         ol[paths[i].getAttribute('title')] = paths[i].getAttribute('d')
      }
   })
</script>

<main>
   <select bind:value={selc}>
      {#each Object.entries(ol) as [t, d]}
      <option value={t} >{t}</option>
      {/each}
   </select>
   <br>
   <hr>
   <svg viewBox="0 0 1009.6727 665.96301">
      {#each Object.entries(ol) as [t, d]}
         <path d={d} title={t} fill={selc == t ? 'rgb(255,200,230)' : 'gray'}></path>
      {/each}
   </svg>
</main>

<style>
  svg {
    width: 1000%;
  }
</style>
