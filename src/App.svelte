<script>
  import { getVersion, createCluster, deleteCluster } from "./beibootctl";

  let promise = getVersion();
  let clusterName;
  let handlerPromise;
  let createPromiseRunning = false;
  let deletePromiseRunning = false;
  let error = "";

  function createClusterHandler() {
    error = "";
    createPromiseRunning = true;
    console.log(clusterName);
    if (clusterName) {
      handlerPromise = createCluster(clusterName).then((output) => {
        createPromiseRunning = false;
        console.log(output);
        if (output.code !== 0) {
          error = output.stderr;
        }
      });
    }
  }

  function deleteClusterHandler() {
    error = "";
    deletePromiseRunning = true;
    console.log(clusterName);
    if (clusterName) {
      handlerPromise = deleteCluster(clusterName).then((output) => {
        deletePromiseRunning = false;
        console.log(output);
        if (output.code !== 0) {
          error = output.stderr;
        }
      });
    }
  }
</script>

<div id="app">
  <img class="logo-light" src="beiboot-logo-light.png" alt="Getdeck Beiboot" />
  <img class="logo-dark" src="beiboot-logo-dark.svg" alt="Getdeck Beiboot" />

  {#await promise}
    <p>Loading…</p>
  {:then version}
    <p>{version}</p>
  {/await}

  <input bind:value={clusterName} />

  <button on:click={createClusterHandler} disabled={createPromiseRunning}
    >Create Cluster</button
  >
  <button on:click={deleteClusterHandler} disabled={deletePromiseRunning}>Delete Cluster</button>

  {#await handlerPromise}
    <p>Cluster gets created/deleted</p>
  {:then clusterOutput}
    {#if error}
      <p>{error}</p>
    {:else}
      <p>Cluster created/created successfully</p>
    {/if}
  {/await}
</div>
