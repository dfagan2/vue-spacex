<template>
  <div class="list">
    <div class="header">
      <div class="badge">Badge</div>
      <div class="name">Rocket Name</div>
      <div class="type">Rocket Type</div>
      <div class="date">Launch Date</div>
      <div class="details">Details</div>
      <div class="id">ID</div>
      <div class="article">Article</div>
    </div>
    <div v-if='launches.length === 0 && !isFetching && !errorMessage' class="no-launches">
      No Launches (..yet)
    </div>
    <div v-if='isFetching' class="loading">
      <div class="loader">
        <div class="inner one"></div>
        <div class="inner two"></div>
        <div class="inner three"></div>
      </div>
    </div>
    <div v-if='errorMessage' class="error">
      {{ errorMessage }}
    </div>
    <ul>
      <li v-for="launch in launches" :key='launch.flight_number'>
        <div class="badge" data-title="Badge:"><img :src="launch.links.mission_patch" /></div>
        <div class="name" data-title="Rocket Name:">{{ launch.rocket.rocket_name }}</div>
        <div class="type" data-title="Rocket Type:">{{ launch.rocket.rocket_type }}</div>
        <div class="date" data-title="Launch Date:">{{ formatter(launch.launch_date_unix) }}</div>
        <div class="details" data-title="Details:">{{ launch.details }}</div>
        <div class="id" data-title="ID:">{{ launch.flight_number }}</div>
        <div class="article" data-title="Article:">
          <a :href="launch.links.article_link" target="_blank">
            <svg viewBox="0 0 512 512">
              <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="../assets/images/link.svg#icon"></use>
            </svg>
          </a>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { format } from 'date-fns'

export default {
  name: 'LaunchesList',
  props: {
    launches: Array,
    isFetching: Boolean,
    errorMessage: String
  },
  methods: {
    formatter: function (date) {
      return format(new Date(date), 'MM/DD/YYYY')
    }
  }
}
</script>

<style scoped lang="scss">
  .header {
    background: linear-gradient(to bottom, #9DB7CD, #ffffff);
    height: 49px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 7px;

    div {
      font-size: 17px;
      color: #043D67;
      padding: 0 1%;
    }
  }

  .error {
    color: #B25757;
    background: linear-gradient(to bottom, #B25757, #ffffff);
    height: 49px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: -7px;
  }

  ul {
    margin: 0;
    padding: 0;
    list-style: none;

    li {
      background: rgba(255,255,255,0.85);
      height: 65px;
      border: 1px solid white;
      border-radius: 8px;
      margin-bottom: 7px;

      display: flex;
      align-items: center;
      justify-content: space-between;

      div {
        padding: 0 1%;
        color: #444444;
        font-size: 15px;
      }
    }
  }

  .badge {
    flex-basis: 10%;

    img {
      max-height: 40px;
    }
  }

  .name {
    flex-basis: 13%;
    text-align: left;
  }

  .type {
    flex-basis: 12%;
    text-align: left;
  }

  .date {
    flex-basis: 12%;
    text-align: left;
  }

  .id {
    flex-basis: 4%;
  }

  .article {
    flex-basis: 9%;

    svg {
      width: 21px;
      height: 21px;
    }
  }

  .details {
    flex-basis: 40%;
    max-width: 325px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    text-align: left;
  }

  .loading {
    position: relative;
  }

  .loader {
    position: absolute;
    top: 30px;
    left: calc(50% - 32px);
    width: 64px;
    height: 64px;
    border-radius: 50%;
    perspective: 800px;
  }

  .inner {
    position: absolute;
    box-sizing: border-box;
    width: 100%;
    height: 100%;
    border-radius: 50%;  
  }

  .inner.one {
    left: 0%;
    top: 0%;
    animation: rotate-one 1s linear infinite;
    border-bottom: 3px solid #EFEFFA;
  }

  .inner.two {
    right: 0%;
    top: 0%;
    animation: rotate-two 1s linear infinite;
    border-right: 3px solid #EFEFFA;
  }

  .inner.three {
    right: 0%;
    bottom: 0%;
    animation: rotate-three 1s linear infinite;
    border-top: 3px solid #EFEFFA;
  }

  @media screen and (max-width: 560px) {
    .header {
      display: none;
    }

    .list {
      margin-top: -5px;
    }

    ul {
      li {
        display: block;
        height: auto;
        padding: 15px;

        div {
          display: flex;
          align-items: center;

          &:not(:last-child) {
            padding-bottom: 10px;
            margin-bottom: 10px;
            border-bottom: 1px solid rgba(0,0,0,0.05);
          }

          &.details {
            white-space: normal;
          }
          
          &:before {
            content: attr(data-title);
            display: block;
            width: 40%;
            text-align: left;
            flex-shrink: 0;

            font-size: 17px;
            color: #043D67;
          }
        }
      }
    }
  }

  @keyframes rotate-one {
    0% {
      transform: rotateX(35deg) rotateY(-45deg) rotateZ(0deg);
    }
    100% {
      transform: rotateX(35deg) rotateY(-45deg) rotateZ(360deg);
    }
  }

  @keyframes rotate-two {
    0% {
      transform: rotateX(50deg) rotateY(10deg) rotateZ(0deg);
    }
    100% {
      transform: rotateX(50deg) rotateY(10deg) rotateZ(360deg);
    }
  }

  @keyframes rotate-three {
    0% {
      transform: rotateX(35deg) rotateY(55deg) rotateZ(0deg);
    }
    100% {
      transform: rotateX(35deg) rotateY(55deg) rotateZ(360deg);
    }
  }
</style>
