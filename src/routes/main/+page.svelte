<script>
  import {goto} from '$app/navigation'
</script>
<div>hi</div>

<button on:click={() => goto('/')}>루트경로로 이동</button>