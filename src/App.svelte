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
      for (let i = 0; i < paths.length; i++) {
         ol[paths[i].getAttribute('title')] = paths[i].getAttribute('d')
      }
   })
</script>

<main>
   <svg viewBox="0 0 1009.6727 665.96301">
      {#each Object.entries(ol) as [t, d]}
         <path d={d} title={t} 
            fill = {selc == t ? 'rgb(200,255,245)' : 'rgb(60,60,60)'}
            stroke-width = {selc == t ? '0.5' : '0.02'}
            stroke = {selc == t ? 'rgb(255,200,230)' : 'black'}
         ></path>
      {/each}
   </svg>
   <select bind:value={selc}>
      {#each Object.entries(ol).sort() as [t, d]}
      <option value={t} >{t}</option>
      {/each}
   </select>
</main>

<style>
   select {
      font-family: 'Times New Roman', Times, serif;
      font-size: 20px;
      position: fixed;
      top: 110px;
   }
   svg {
         width: 100%;
         position: absolute;
         left: 0px;
         top: 100px;
   }
</style>
