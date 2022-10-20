<script>
  import {goto} from '$app/navigation'
</script>
<div>
  <button on:click={() => goto('/')}>루트경로로 이동</button>
  <div class="box">
    <slot>
      <!-- <span>hi</span> -->
    </slot>
   
  </div>

</div>

<style>
  .box {
    border: 1px solid #000;
  }
</style>