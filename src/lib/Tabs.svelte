<script>
  import { quintOut } from "svelte/easing";
  import { fly, fade, slide, crossfade } from "svelte/transition";
  import { flip } from "svelte/animate";
  import { onMount, afterUpdate, beforeUpdate } from "svelte";

  import {
    icon_refresh,
    icon_menu,
    icon_list,
    icon_yet,
    icon_know,
  } from "./icons.js";
  import { btnStyle, subBtnStyle, footBtnStyle } from "./style.js";
  import Drawer from "svelte-drawer-component";
  //
  let open = false;

  export let tabs = [
    { label: "Curriculum", icon: icon_list, value: 1 },
    { label: "Notes", icon: icon_know, value: 2 },
  ];

  export let activeTabValue = 1;
  export let tabRight = true;
  export const handleClick = (tabValue) => {
    tabValue > activeTabValue ? (tabRight = true) : (tabRight = false);
    if (tabValue === 2) {
      // curr_yets = JSON.parse(localStorage.getItem("notes"));
    }

    return (activeTabValue = tabValue);
  };

  export let curri = ["1", "1", "1", "1", "1", "1", "1"];
  export let notes = ["1", "1", "1", "1", "1", "1", "1"];

  // export let selected;

  // Save
  $: localStorage.setItem("curri", JSON.stringify(curri));
  $: localStorage.setItem("notes", JSON.stringify(notes));

  let curri_btn_status = {};
  function curri_btn_toggle(index) {
    !curri_btn_status[index]
      ? (curri_btn_status[index] = true)
      : (curri_btn_status[index] = false);
  }
  let note_btn_status = {};
  function note_btn_toggle(index) {
    !note_btn_status[index]
      ? (note_btn_status[index] = true)
      : (note_btn_status[index] = false);
  }

  onMount(() => {
    console.log("mount!");
  });
</script>

<div class="topTabs w-screen fixed top-0 left-0 right-0 z-50 backdrop-blur">
  <ul class="mt-2 ml-2 text-l text-slate-700">
    <button
      on:click={() => (open = true)}
      class="ml-2 mr-4 hover:-rotate-90 active:scale-110 active:-rotate-90 transition-all ease-in-out duration-300"
      ><svg
        xmlns="http://www.w3.org/2000/svg"
        class="inline-block h-6 w-6"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d={icon_menu}
        />
      </svg>
    </button>

    {#each tabs as tab}
      <li class={activeTabValue === tab.value ? "active" : ""}>
        <span class="tabs" on:click={handleClick(tab.value)}>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="inline h-6 w-6"
            fill="none"
            viewBox="2 2 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d={tab.icon}
            />
          </svg>
          {tab.label}
        </span>
      </li>
    {/each}
  </ul>
</div>

<div class="mb-14">
  {#each tabs as tab}
    {#if activeTabValue == tab.value}
      <div
        class="pt-9"
        in:fly={tabRight
          ? {
              delay: 0,
              duration: 300,
              x: 500,
              opacity: 1,
              easing: quintOut,
            }
          : {
              delay: 0,
              duration: 300,
              x: -500,
              opacity: 1,
              easing: quintOut,
            }}
      >
        <!-- Tab -->

        <div class="grid pt-4 h-full break-words">
          <Drawer size="400px" {open} on:clickAway={() => (open = false)}>
            <div class="grid-cols-2">
              <div class="absolute left-8 top-5 text-2xl border-b-2">
                Qick: JavaScript
              </div>
              <button
                on:click={() => (open = false)}
                class="ml-2 mr-4 rotate-90 hover:-rotate-0 active:scale-110 active:-rotate-0 transition-all ease-in-out duration-300 absolute right-2 top-6 "
                ><svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="inline-block h-6 w-6"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d={icon_menu}
                  />
                </svg></button
              >
            </div>
            <div class="absolute top-20 left-8 space-y-4">
              <div class="{btnStyle} text-xl w-80">menu 1</div>
              <div class="{btnStyle} text-xl w-80">menu 1</div>
              <div class="{btnStyle} text-xl w-80">menu 1</div>
              <div class="{btnStyle} text-xl w-80">menu 1</div>
              <div class="{btnStyle} text-xl w-80">menu 1</div>
            </div>
            <div class="absolute bottom-2 right-4">version 1.0.0</div>
          </Drawer>

          <!-- Tab1 -->
          {#if tab.value === 1}
            <div class="tab1Page px-4 pt-4 space-y-2">
              <div class="grid mb-4 ">
                {#each curri as check}
                  <div in:slide>
                    <div class="block text-left " transition:slide|local>
                      <div class="mb-2">
                        <div
                          class="flex {btnStyle} grid text-lg"
                          in:fly|local={{ duration: 300 }}
                          out:fade|local={{ duration: 100 }}
                        >
                          <div class="mb-2 font-semibold text-xl border-b-2 ">
                            <h2 class="text-2xl pb-2 mb-2 border-b-2">
                              {check} 안녕하세요
                            </h2>
                            <div class="text-xl">
                              {check}
                            </div>
                            <div class="text-lg">
                              {check}
                            </div>
                            <div class="text-base">
                              {check}
                              {check}
                            </div>

                            {check}
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                {/each}
              </div>
            </div>
          {/if}

          <!-- Tab1 -->

          <!-- Tab2 -->
          {#if tab.value === 2}
            <div class="tab2Page px-4">
              <!-- <div class="grid pt-2 mb-4 ">
                {#each notes as yet, i (yet)}
                  <div in:slide>
                    <div class="block text-left " transition:slide|local>
                      <div class="mb-2">
                        <div
                          class="{btnStyle} py-3 grid text-lg"
                          in:fly|local={{ duration: 300 }}
                          out:fade|local={{ duration: 100 }}
                        >
                          <button
                            class="text-left w-full"
                            on:click={() => {
                              note_btn_toggle(i);
                            }}
                          >
                            <div class="font-semibold text-xl border-b-2">
                              {yet}
                            </div>

                            <div class="space-y-2 mt-2 text-center">
                              {#if note_btn_status[i]}
                                <div transition:slide|local={{ duration: 200 }}>
                                  <div class="{subBtnStyle} text-base py-1">
                                    I know!
                                  </div>
                                </div>
                              {/if}
                            </div>
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                {/each}
              </div> -->
            </div>
          {/if}
          <!-- Tab 2 -->
        </div>
      </div>
    {/if}
  {/each}
</div>

<style>
  /* Tabs */
  ul {
    display: flex;
    flex-wrap: wrap;
    padding-left: 0;
    margin-bottom: 0;
    list-style: none;
    border-bottom: 1px solid #dee2e6;
  }
  li {
    width: 40vw;
    margin-bottom: -1px;
    text-align: center;
    font-weight: 700;
  }
  /* .topTabs {
    overflow-x: hidden;
    overflow-y: hidden;
  } */
  span.tabs {
    border: 1px solid transparent;
    border-top-left-radius: 0.25rem;
    border-top-right-radius: 0.25rem;
    display: block;
    padding: 1vh;
    cursor: pointer;
  }

  span.tabs:hover {
    border-color: #e9ecef #e9ecef #dee2e6;
  }

  li.active > span.tabs {
    color: #495057;
    background-color: #fff;
    border-color: #dee2e6 #dee2e6 #fff;
  }
  @keyframes bounce {
    0%,
    100% {
      transform: translateY(-12%);
      animation-timing-function: cubic-bezier(0.8, 0, 1, 1);
    }
    50% {
      transform: translateY(+5%);
      animation-timing-function: cubic-bezier(0, 0, 0.2, 1);
    }
  }
  .animate-bounce {
    animation: bounce 0.7s infinite;
  }

  .tab1Page,
  .tab2Page {
    overflow-y: scroll;
    height: 94vh;
    padding-top: 1vh;
    padding-bottom: 5vh;
  }
</style>
