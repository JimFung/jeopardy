<script lang='ts'>
  import { onMount } from 'svelte';
  import Modal from './Modal.svelte';
  import Drawer from './Drawer.svelte';

  interface question {
    text: string;
    answered: boolean;
    answer: string;
    value: number;
    id: number;
  }

  interface Category {
    topic: string;
    questions: question[];
  }

  interface Team {
    id: number,
    score: number,
  }

  let categories: Category[] = [];
  let teams: Team[] = []

  onMount(() => {
    if (localStorage.getItem('categories')) {
      categories = JSON.parse(localStorage.getItem('categories'));
    } else {
      categories = [
        {
          topic: 'Chrismas Movies',
          questions: [
            {
              id: 1,
              text:
                'In Polar Express, What is the primary mode of transportation',
              answered: false,
              answer: 'train',
              value: 200,
            },
            {
              id: 2,
              text: 'What did the Grinch use to subsitute for reindeer',
              answered: false,
              answer: 'his dog',
              value: 400,
            },
            {
              id: 3,
              text:
                'In the movie Home Alone 2, what state does Kevin\'s family go to on vacation',
              answered: false,
              answer: 'Florida',
              value: 600,
            },
            {
              id: 4,
              text:
                'In A Charlie Brown Christmas, who explains the meaning of Christmas',
              answered: false,
              answer: 'Linus',
              value: 800,
            },
          ],
        },
        {
          topic: 'Seasonal Facts',
          questions: [
            {
              id: 5,
              text:
                'In 1915, this gift card company introduced the very first Christmas cards',
              answered: false,
              answer: 'Hallmark',
              value: 200,
            },
            {
              id: 6,
              text:
                'The largest selling Christmas toy in 1980, this toy now has entire tournaments dedicated to it',
              answered: false,
              answer: 'Rubiks Cube',
              value: 400,
            },
            {
              id: 7,
              text:
                'This American State was the first to recognize Christmas as a holiday',
              answered: false,
              answer: 'Alabama',
              value: 600,
            },
            {
              id: 8,
              text:
                'Boston, Massachusetts gets a Christmas tree from this Canadian province every year',
              answered: false,
              answer: 'Nova Scotia',
              value: 800,
            },
          ],
        },
        {
          topic: 'Chrismas Carols',
          questions: [
            {
              id: 9,
              text: 'This reindeer is famous for their brightly coloured nose',
              answered: false,
              answer: 'rudolph',
              value: 200,
            },
            {
              id: 10,
              text: 'What fun it is to ride in a one horse open sleigh',
              answered: false,
              answer: 'jingle bells',
              value: 400,
            },
            {
              id: 11,
              text:
                'This jolly happy soul, is brought to life by a discarded magical hat',
              answered: false,
              answer: 'Frosty the snowman',
              value: 600,
            },
            {
              id: 12,
              text: 'On the 1th day of Christmas, this was given',
              answered: false,
              answer: '11 Pipers Piping',
              value: 800,
            },
          ],
        },
        {
          topic: 'Christmas in B.C.',
          questions: [
            {
              id: 13,
              text:
                'One of Vancouver\'s most iconic attractions, You can travel around it on train to see the Christmas lights every December',
              answered: false,
              answer: 'Stanley Park',
              value: 200,
            },
            {
              id: 14,
              text:
                'Known for its flora and hedge maze, This location annual festival of lights is a popular attraction',
              answered: false,
              answer: 'VanDusen Botanical Garden',
              value: 400,
            },
            {
              id: 15,
              text:
                'A site of olympic pride, This public space is turned into a festive market every year',
              answered: false,
              answer: 'Jack Poole Plaza',
              value: 600,
            },
            {
              id: 16,
              text:
                'Located in the sixth largest city in B.C., The lights set up at this location draw crowds from over the lower mainland.',
              answered: false,
              answer: 'Lafarge Lake',
              value: 800,
            },
          ],
        },
        {
          topic: 'Chrismas Babies',
          questions: [
            {
              id: 17,
              text:
                'This Canadian politician is a member of the Liberal party and born in the year 1971',
              answered: false,
              answer: 'Justin Trudeau',
              value: 200,
            },
            {
              id: 18,
              text:
                'Born in the year 1642, he would go on to be a defining figure in multiple fields including Mathematics and Physics',
              answered: false,
              answer: 'Sir Isaac Newton',
              value: 400,
            },
            {
              id: 19,
              text:
                'Born 1899 in New York City. This actor is best known his role in the 1942 film, Casablanca',
              answered: false,
              answer: 'Humphrey Bogart',
              value: 600,
            },
            {
              id: 20,
              text:
                'Born 1958 in Chicago. He would go on to be a 10 time MLB All star and holds the record MLB record for stolen bases',
              answered: false,
              answer: 'Rickey Henderson',
              value: 800,
            },
          ],
        },
      ];
    }
    
    if (localStorage.getItem('teams')) {
      teams = JSON.parse(localStorage.getItem('teams'))
    }
  });

  let modalData: question = null;
  let hideAnswer = true;

  function setAnswered(id: number) {
    for (const category of categories) {
      for (const q of category.questions) {
        if (q.id === id) {
          q.answered = true;
          categories = categories;
          localStorage.setItem('categories', JSON.stringify(categories));
          return;
        }
      }
    }
  }

  let drawerOpen = false;

  function resetBoard() {
    for (const category of categories) {
      for (const q of category.questions) {
        q.answered = false;
      }
    }
    categories = categories;
    teams = [];
    localStorage.setItem('categories', JSON.stringify(categories));
    localStorage.setItem('teams', JSON.stringify([]));
  }

  function addTeam() {
    teams = [...teams, { id: teams.length + 1, score: 0 }];
    localStorage.setItem('teams', JSON.stringify(teams));
  }

  function increaseScore(index, value) {
    teams[index].score = teams[index].score + value;
    teams = teams;
    localStorage.setItem('teams', JSON.stringify(teams));
  }
</script>

{#if modalData}
  <Modal
    on:close={() => {
      modalData = null;
      hideAnswer = true;
    }}>
    <section
      class='h-full flex flex-col justify-center items-center bg-blue-700 text-yellow-300'
      slot='content'>
      <h2 class='font-sans font-bold text-4xl text-center w-4/6 mb-6'>
        {modalData.text}
      </h2>
      <h3 class={`mb-12 text-3xl ${hideAnswer ? 'text-blue-700' : ''}`}>
        {modalData.answer}
      </h3>
      <button
        class='border px-4 py-2 mb-3 rounded'
        on:click={() => {
          hideAnswer = false;
          setAnswered(modalData.id);
        }}>Show Answer</button>
      <div class='flex w-3/6 space-x-2 mb-3 justify-center'>
        {#each teams as team, i}
          <button
          class='border px-4 py-2 rounded'
          on:click={() => {
            setAnswered(modalData.id);
            increaseScore(i, modalData.value);
            modalData = null;
            hideAnswer = true;
          }}>Team {team.id}</button>
        {/each}
      </div>
      <button
        class='border px-4 py-2 rounded'
        on:click={() => {
          modalData = null;
          hideAnswer = true;
        }}>close</button>
    </section>
  </Modal>
{/if}

{#if drawerOpen}
  <Drawer on:close={() => drawerOpen = false} bind:open={drawerOpen}>
    <div class='bg-blue-800 h-screen py-8 shadow-2xl'>
      <h2 class='text-yellow-200 text-3xl font-sans font-medium px-4 mb-6'>Options</h2>
      <ul>
        <li>
          <button class='w-full flex items-center text-left px-4 py-2 mb-4 text-yellow-200 text-2xl font-sans font-light hover:bg-yellow-200 hover:text-blue-800' on:click={() => { resetBoard(); drawerOpen = false; }}>
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path></svg>
            <span class='ml-4'>Reset Game</span>
          </button>
        </li>
      </ul>
      <hr class='mx-auto w-5/6' />
      <div class='flex flex-col p-4'>
        <div class='flex text-yellow-200'>
          <h2 class='flex-grow text-lg'>Teams</h2>
          <button class='flex items-center' on:click={addTeam}>
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6"></path></svg>
            Add Team
          </button>
        </div>
        {#each teams as team}
          <div class='flex flex-col w-full p-4 mt-4 border-2 rounded bg-blue-600 text-yellow-200 border-yellow-200'>
            <p>Team {team.id}</p>
            <p>Score: {team.score}</p>
          </div>
        {/each}
      </div>
    </div>
  </Drawer>
{/if}

<main class='h-screen bg-blue-700 flex flex-col px-4 pb-4'>
  <div class='flex'>
    <h1 class='text-yellow-300 font-bold font-sans text-4xl text-center py-4 flex-grow'>
      J E O P A R D Y !
    </h1>
    <button class='text-yellow-300' on:click={() => drawerOpen = true} ><svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path></svg></button>
  </div>
  <section class='flex flex-grow'>
    {#each categories as { topic, questions }}
      <section class='parent flex-grow h-full border text-yellow-300'>
        <div class='flex items-center justify-center border'>
          <h2 class='text-2xl'>{topic}</h2>
        </div>
        {#each questions as q}
          <button
            class={`flex items-center justify-center border text-3xl ${q.answered ? 'text-blue-700' : ''}`}
            on:click={() => (modalData = q)}
            disabled={q.answered}>
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