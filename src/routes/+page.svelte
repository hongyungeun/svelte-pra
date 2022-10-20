<script>


  import {goto} from '$app/navigation'
  import axios from 'axios'
  import {onMount, onDestroy,afterUpdate} from 'svelte'
	import { dataset_dev } from 'svelte/internal';
  afterUpdate(()=>{
    let bindTest = document.getElementById('bind').innerText
    console.log(bindTest)
  })
  
  let userNum = 1
  let api = axios.create({
    baseURL : `https://jsonplaceholder.typicode.com/users`,
    headers:{'Content-type':'application/json'}
  })

  let focus;

  onMount(()=>{
    focus.focus()
    call()
    
  })
  onDestroy(()=>{
    clearInterval(timeInit)
  })
  let countNum = Math.floor(0)
  let array = [1,2,3,4,5,6]
  let count = 0
  $: doubleCount = count + countNum
  function countif(e){
    console.log(countNum)
    e = event.target.value
    if(e === '증가'){
      if(countNum == 0){
        count++
        console.log('hi')
      }else {
        count += Number(countNum)
        console.log('hi2')
      }
    }else { 
      if(countNum == 0 ){
        count--
      }else {
        count-=countNum
      }
    }
  }
  


  const TRAFFIC_LIGHTS = ['red', 'orange', 'green'];
	let lightIndex = 0;

	$: light = TRAFFIC_LIGHTS[lightIndex];

	function nextLight() {
		if (lightIndex + 1 > TRAFFIC_LIGHTS.length - 1) {
			lightIndex = 0;
		} else {
			lightIndex++;
		}
	}
  



  
	let pageTitle = '';
	onMount(() => {
		pageTitle = document.title;
	});


  let isAvailable = true;


  let picked = 'black';

  let inputVal = 'hi'




  const list = [
    { id: 1, text: 'A', id2: 10 },
    { id: 2, text: 'B', id2: 20 },
    { id: 3, text: 'C', id2: 30 }
  ]

  let selected


  let url = `https://jsonplaceholder.typicode.com/users`
  let callback
  async function call(){
    
      let response = await fetch(url).
      then(res=> res.json())
      console.log(response)
      // callback = response
      callback = response.map(item=>item)
      console.log(callback)
      // let json = response.json()
      
    
  }


  let time = 1;
  let timeInit
  let unmountTime

  let intervalTime = function(){
    timeInit = setInterval(() => {
      console.log(time++)
    }, 1000);
  }

  let axiosCallback
  let axiosCall = async ()=>{
    let response = await api.get('/1') 
    axiosCallback = response.data
    console.log(axiosCallback,'callback1')
  }

  async function otherUserCall(item){
    
    let response = await api.get(`/${item}`) 
    axiosCallback = response.data
    console.log(axiosCallback,'callback3')
    console.log(userNum)
    
  }

</script>

<svelte:head>
	<title>main</title>
	<meta name="description" content="mainPage" />
</svelte:head>

<div class="main">
  
  <p>hi</p>
  {#each array as item }
    <p>{item}</p>
  {/each}
  <!-- <p>{Math.floor(count+countNum)}</p> -->
  <p>{Math.floor(count)}</p>
  <input type="text" bind:this={focus} class="input_num" bind:value={countNum}>
  <input type="button" on:click={countif} value="증가">
  <input type="button" on:click={countif} value="감소">
  <br>
  <br>
  <br>
  <button on:click={nextLight}>Next light</button>
  <p>Light is: {light}</p>
  <p>
    You must
    {#if light === 'red'}
      <span>STOP</span>
    {:else if light === 'orange'}
      <span>SLOW DOWN</span>
    {:else if light === 'green'}
      <span>GO</span>
    {/if}
  </p>

  <p>Page title is: {pageTitle}</p>



  <input id="is-available" type="checkbox" bind:checked={isAvailable} />
  <label for="is-available">Is available</label>



  <div>Picked: {picked}</div>

  <input id="blue-pill" bind:group={picked} type="radio" value="blue" />
  <label for="blue-pill">Blue pill</label>

  <input id="red-pill" bind:group={picked} type="radio" value="red" />
  <label for="red-pill">Red pill</label>

  <input id="black-pill" bind:group={picked} type="radio" value="black" />
  <label for="black-pill">Black pill</label>

  <input type="text" bind:value={inputVal}>
  <input type="text" bind:value={inputVal}>
  <input type="text" bind:value={inputVal}>


  <select bind:value="{selected}">
    {#each list as item  }
      <option value="{item}">{item.text}</option>
    {/each}
  </select>
  <span>선택함: {selected ? selected.id2 : 'waiting...'}</span>
  <div>
    {#if selected?.id === 1}
    <span>hi</span>
    {:else if selected?.id === 2}
    <span>hi2</span>
    <span>hi3</span>
    {:else if selected?.id ===3}
    <span>hi3</span>
    {/if}
  </div>
  

  <button on:click={call}>fetchCall</button>
  <div>{callback == undefined ? 'waiting...':callback.map(item=>item.id)}</div>
  <div>{callback == undefined ? '':callback.map(item=>item?.name)}</div>
  <!-- <div>{callback?.id}</div> -->


  <button on:click={intervalTime}>lifeCycle test 버튼누르면 시작</button>
  <button on:click={() => goto('/main')}>메인으로 이동</button>

  <div id="bind" >바인드다</div>

  <div>Axios예제</div>
  <button on:click={axiosCall}>axiosCall</button>
  <button on:click={()=>{userNum++
    otherUserCall(userNum)}}>이거누르면 다른유저 불러옴</button>
  <div>
    {axiosCallback ? axiosCallback.id: '이거아닌듯;'}
    {#if axiosCallback}
    {axiosCallback.id}
    {/if}
  </div>
</div>




<style>

</style>
