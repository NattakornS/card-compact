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
  
  <canvas bind:this={barcodeCanvas}></canvas>