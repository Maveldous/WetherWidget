<template>
  <div id="app">
    <article class="ref__description article">
        <h1 class="ref__description-title title">Wether plugin</h1>
        <p class="ref__description-text">Here is a component wether plugin written in a Vue framework. It is fully responsive and intuitive. With the ability to embed it anywhere without additional dependencies. Customizable to the needs of the designer.</p>
    </article>
    <article class="ref__demo article">
        <h1 class="ref__demo-title title">Demonstration</h1>
        <wetherPlugin country="Kyiv" />
      </article>
      <article class="ref__documentation article">
          <h1 class="ref__documentation-title title">Documentation</h1>
          <table class="ref__documentation-table">
              <thead>
                  <tr>
                      <th>Parameter</th>
                      <th>Type</th>
                      <th>Description</th>
                  </tr>
              </thead>
              <tbody>
                  <tr>
                      <td class="red">country</td>
                      <td>String</td>
                      <td>The name of the place where the weather forecast will be</td>
                  </tr>
              </tbody>
          </table>
      </article>
    </div>
</template>


<script>
import wetherPlugin from "./components/wetherPlugin.vue";

export default {
  name: "App",
  components: {
    wetherPlugin
  }
};
</script>

<style>
    body{
        font-family: 'Roboto', sans-serif;
    }
    .article{
        padding: 30px;
    }
    .title{
        display: block;
        font-size: 50px;
        font-weight: 700;
    }
    .ref__description-text{
        display: inline-block;
        max-width: 500px;
        color: #888;
    }

    tr{
        margin: 50px 0;
    }

    th, td{
        width: 200px;
        height: 70px;
        text-align: left;
    }

    .red{
        color: red;
    }

    @media (max-width: 600px){
        .article{
            padding: 0px;
        }
        .title{
            font-size: 40px;
        }
    }
</style>