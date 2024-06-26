<script lang="ts" setup>
import {
  CharacterDetails,
  CharacterFilms,
  CharacterSpecie,
  CharacterStarships,
  CharacterVehicles,
} from '#components'
import { ArrowRight, ArrowLeft } from 'lucide-vue-next'
import type { DefineComponent } from 'vue';

import type { PeopleType } from '~/types/people'

const route = useRoute()
const router = useRouter()

const { data } = await useFetch<PeopleType>(`https://swapi.dev/api/people/${route.params.id}`)

const DEFAULT_TAB = 'details'
const currentTab = ref<string>(DEFAULT_TAB)




const tabs = [
  {
    label: 'Detalhes',
    value: 'details'
  },
  {
    label: 'Filmes',
    value: 'films'
  },
  {
    label: 'Espécie',
    value: 'specie'
  },
  {
    label: 'Veículos',
    value: 'vehicles'
  },
  {
    label: 'Naves espaciais',
    value: 'starships'
  },
]

const avaliableTabs = computed(() => {
  const { species = [], starships = [], vehicles = [] } = data.value!
  const tabsToFilter = ['specie', 'vehicles', 'starships']

  interface HasData {
    specie: boolean
    starships: boolean
    vehicles: boolean
  }

  const hasData: HasData = {
    specie: !!species?.length,
    starships: !!starships?.length,
    vehicles: !!vehicles?.length
  }

  return tabs.filter(({ value }) => {
    // Caso não exista na lista de abas a serem filtradas então deve sempre existir.
    if (!tabsToFilter.includes(value)) return true

    return hasData[value as keyof HasData]
  })
})

const currentTabIndex = computed(() => {
  const tabsValue = avaliableTabs.value.map(({ value }) => value)

  return tabsValue.findIndex(value => value === currentTab.value)
})

function goToNextTab() {
  const nextTabIndex = currentTabIndex.value === (avaliableTabs.value.length - 1) ? 0 : currentTabIndex.value + 1

  console.log({ avaliableTabs, currentTabIndex, nextTabIndex })
  setTab(avaliableTabs.value[nextTabIndex].value)
}

function goToPreviousTab() {
  const previousTabIndex = currentTabIndex.value === 0 ? avaliableTabs.value.length - 1 : currentTabIndex.value - 1

  setTab(avaliableTabs.value[previousTabIndex].value)
}

function getActivateTabClass(tab: string) {
  return { 'character-page__tab-activate': tab === currentTab.value }
}

function setTab(tab: string) {
  let validatedTab = tab

  // Caso seja informada uma aba inválida, será definida uma aba padrão.
  if (!tabs.some(({ value }) => value === tab)) {
    validatedTab = DEFAULT_TAB
  }

  currentTab.value = validatedTab

  router.push({
    query: {
      ...route.query,
      tab: validatedTab
    }
  })
}

watch(() => route.query.tab, (value) => {
  router.replace({ query: { tab: value } })
})

const component = computed(() => {
  interface ComponentProps {
    is: DefineComponent
    /* eslint-disable-next-line */
    props: any
  }

  interface Components {
    details: ComponentProps
    films: ComponentProps
    specie: ComponentProps
    starships: ComponentProps
    vehicles: ComponentProps
  }

  const components = {
    details: {
      is: CharacterDetails,
      props: {
        details: {
          height: data.value?.height ?? '',
          mass: data.value?.mass ?? '',
          hair_color: data.value?.hair_color ?? '',
          skin_color: data.value?.skin_color ?? '',
          eye_color: data.value?.eye_color ?? ''
        }
      }
    },
    films: {
      is: CharacterFilms,
      props: { films: data.value?.films }
    },
    specie: {
      is: CharacterSpecie,
      props: {
        specieUrl: data.value?.species[0]
      }
    },
    vehicles: {
      is: CharacterVehicles,
      props: { vehicles: data.value?.vehicles }
    },
    starships: {
      is: CharacterStarships,
      props: { starships: data.value?.starships }
    },
  }

  return components[currentTab.value as keyof Components]
})

setTab(route.query?.tab as string || 'details')
</script>

<template>
  <div class="character-page">
    <header class="character-page__header">
      <h1 class="character-page__title">Detalhes do personagem</h1>
      <h2 class="character-page__name">{{ data?.name }}</h2>
    </header>

    <nav>
      <ul class="character-page__tab-list">
        <li v-for="tab of avaliableTabs" :key="tab.value" :class=getActivateTabClass(tab.value)
          class="character-page__tab" @click="setTab(tab.value)">
          {{ tab.label }}
        </li>
      </ul>
    </nav>

    <div class="character-page__wrap-details">
      <button class="character-page__button-left" @click="goToPreviousTab">
        <ArrowLeft />
      </button>

      <div class="character-page__container">
        <NuxtImg class="character-page__image" loading="lazy" src="/images/star-wars.png" height="320px"
          width="162px" />

        <div class="character-page__container-details">
          <Transition mode="out-in">
            <component :is='component.is' v-bind="component.props" />
          </Transition>
        </div>
      </div>

      <button class="character-page__button-right" @click="goToNextTab">
        <ArrowRight />
      </button>
    </div>

    <AppLink path="/" text="Voltar aos outros personagens" />
  </div>
</template>

<style lang="scss" scoped>
.character-page {
  background-image: url("~/public/images/background.png");
  display: flex;
  flex-direction: column;
  gap: 72px;
  padding: 72px 72px;

  &__header {
    text-align: center;
  }

  &__title {
    font-size: 2rem;
  }

  &__name {
    color: $primary;
    font-size: 4.5rem;
    margin-top: 48px;
  }

  &__tab-list {
    display: flex;
    flex-wrap: wrap;
    gap: 32px;
    justify-content: center;
    list-style: none;
  }

  &__tab {
    background-color: #151515;
    border: solid 1px $secondary;
    border-radius: 32px;
    color: $secondary;
    cursor: pointer;
    font-size: 1.5rem;
    padding: 12px 48px;

    &-activate {
      background-color: $secondary;
      color: black;
    }
  }

  &__wrap-details {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__container {
    display: flex;
    align-items: center;
    gap: 24px;
    position: relative;
    padding: 28px 0 28px 40px;

    &::before {
      content: "";
      top: 0;
      left: 0;
      position: absolute;
      height: 100%;
      width: 700px;
      background-color: #DBA90D;
      border-radius: 24px;
    }
  }

  &__image {
    filter: brightness(115%)
  }

  &__container-details {
    background-color: #151515;
    border-radius: 24px;
    height: 432px;
    padding: 32px;
    width: 628px;
    z-index: 1;
  }

  &__button-left,
  &__button-right {
    background-color: #A5A29A;
    border-radius: 35px;
    border: none;
    cursor: pointer;
    height: 70px;
    outline: none;
    transition: 300ms ease-in-out;
    width: 70px;

    &:hover {
      background-color: $secondary;
    }
  }
}

.v-enter-active,
.v-leave-active {
  transition: all 0.3s ease;
}

.v-enter-from {
  transform: translateX(-20px);
  opacity: 0;
}

.v-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
</style>
