<script>
    import { onMount, afterUpdate } from 'svelte';
    import JsBarcode from 'jsbarcode';
  
    export let data = '';
  
    let barcodeCanvas;
  
    onMount(() => {
      JsBarcode(barcodeCanvas, data);
    });
  
    afterUpdate(() => {
      JsBarcode(barcodeCanvas, data);
    });
  </script>
  <div class="canvas-container">
    <canvas bind:this={barcodeCanvas} ></canvas>
  </div>
  <style>
    .canvas-container {
      width: auto;
      height: 100%;
    }
  
    .canvas-container canvas {
      width: 100%;
      height: 100%;
    }
  </style>