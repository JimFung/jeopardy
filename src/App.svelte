<script lang="ts">
  import Modal from './Modal.svelte';

  interface question {
    text: string,
    answered: boolean,
    answer: string,
    value: number
  }

  const questions = [
    {
      topic: 'Chrismas Movies',
      questions: [
        {
          text: 'In Polar Express, What is the primary mode of transportation',
          answered: false,
          answer: 'train',
          value: 200
        },
        {
          text: 'What did the Grinch use to subsitute for reindeer',
          answered: false,
          answer: 'his dog',
          value: 400
        },
        {
          text: 'In the movie Home Alone 2, what state does Kevin\'s family go to on vacation',
          answered: false,
          answer: 'Florida',
          value: 600
        },
        {
          text: 'In A Charlie Brown Christmas, who explains the meaning of Christmas',
          answered: false,
          answer: 'Linus',
          value: 800
        },
      ]
    },
    {
      topic: 'Seasonal Facts',
      questions: [
        {
          text: 'In 1915, this gift card company introduced the very first Christmas cards',
          answered: false,
          answer: 'Hallmark',
          value: 200
        },
        {
          text: 'The largest selling Christmas toy in 1980, this toy now has entire tournaments dedicated to it',
          answered: false,
          answer: 'Rubiks Cube',
          value: 400
        },
        {
          text: 'This American State was the first to recognize Christmas as a holiday',
          answered: false,
          answer: 'Alabama',
          value: 600
        },
        {
          text: 'Boston, Massachusetts gets a Christmas tree from this Canadian province every year',
          answered: false,
          answer: 'Nova Scotia',
          value: 800
        },
      ]
    },
    {
      topic: 'Chrismas Carols',
      questions: [
        {
          text: 'This reindeer is famous for their brightly coloured nose',
          answered: false,
          answer: 'rudolph',
          value: 200
        },
        {
          text: 'What fun it is to ride in a one horse open sleigh',
          answered: false,
          answer: 'jingle bells',
          value: 400
        },
        {
          text: 'This jolly happy soul, is brought to life by a discarded magical hat',
          answered: false,
          answer: 'Frosty the snowman',
          value: 600
        },        
        {
          text: 'On the 1th day of Christmas, this was given',
          answered: false,
          answer: '11 Pipers Piping',
          value: 800
        },
      ]
    },
    {
      topic: 'Christmas in B.C.',
      questions: [
        {
          text: 'One of Vancouver\'s most iconic attractions, You can travel around it on train to see the lights every Christmas',
          answered: false,
          answer: 'Stanley Park',
          value: 200
        },
        {
          text: 'Known for its flora and hedge maze, This location annual festival of lights is a popular attraction',
          answered: false,
          answer: 'VanDusen Botanical Garden',
          value: 400
        },
        {
          text: 'A site of olympic pride, This public space is turned into a festive market every year',
          answered: false,
          answer: 'Jack Poole Plaza',
          value: 600
        },
        {
          text: 'Located in the sixth largest city in B.C., The lights set up at this location draw crowds from over the lower mainland.',
          answered: false,
          answer: 'Lafarge Lake',
          value: 800
        },
      ]
    },
    {
      topic: 'Chrismas Babies',
      questions: [
        {
          text: 'This Canadian politician is a member of the Liberal party and born in the year 1971',
          answered: false,
          answer: 'Justin Trudeau',
          value: 200
        },
        {
          text: 'Born in the year 1642, he would go on to define multiple fields including Mathematics and Physics',
          answered: false,
          answer: 'Sir Isaac Newton',
          value: 400
        },
        {
          text: 'Born 1899 in New York City. This actor is best known his role in the 1942 film, Casablanca',
          answered: false,
          answer: 'Humphrey Bogart',
          value: 600
        },
        {
          text: 'Born 1958 in Chicago. He would go on to be a 10 time MLB All star and holds the record MLB record for stolen bases',
          answered: false,
          answer: 'Rickey Henderson',
          value: 800
        },
      ]
    },
  ];

  let modalData: question = null;
  let hideAnswer = true;
</script>

{#if modalData}
  <Modal on:close={() => { modalData = null; hideAnswer = true; }}>
    <section class='h-full flex flex-col justify-center items-center bg-blue-700 text-yellow-300' slot='content'>
      <h2 class='font-sans font-bold text-4xl text-center w-4/6 mb-6'>{modalData.text}</h2>
      <h3 class={`mb-6 text-3xl ${hideAnswer ? 'text-blue-700' : ''}`}>{modalData.answer}</h3>
      <button class='border px-4 py-2 mb-3 rounded' on:click={() => hideAnswer = false}>Show Answer</button>
      <button class='border px-4 py-2 rounded' on:click={() => { modalData = null; hideAnswer = true; }}>close</button>
    </section>
  </Modal>
{/if}

<main class='h-screen bg-blue-700 flex flex-col px-4 pb-4'>
  <h1 class='text-yellow-300 font-bold font-sans text-4xl text-center py-4'>J E O P A R D Y !</h1>
  <section class='flex flex-grow'>
    {#each questions as { topic, questions }}
    <section class='parent flex-grow h-full border text-yellow-300'>
      <div class='flex items-center justify-center border'>
        <h2 class='text-2xl'>{topic}</h2>
      </div>
      {#each questions as q}
        <button class='flex items-center justify-center border' on:click={() => modalData = q} disabled={q.answered}>
          ${q.value}
        </button>
      {/each}
    </section>
  {/each}
  </section>
</main>

<style>
  .parent {
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: 100px repeat(4, 1fr); 
    grid-column-gap: 0px;
    grid-row-gap: 0px;
  }
</style>