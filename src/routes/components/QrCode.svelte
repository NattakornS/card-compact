<script>
  import { onMount } from 'svelte';
  import QRCode from 'qrcode';

  export let data = '';

  let qrCodeUrl = '';

  onMount(async () => {
    try {
      qrCodeUrl = await QRCode.toDataURL(data+'');
    } catch (err) {
      console.error(err);
    }
  });
</script>

<img src={qrCodeUrl} alt="QR Code" />