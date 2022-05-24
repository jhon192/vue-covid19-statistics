<template>
  <div>
    <div class="m-2">
      <div class="p-4">
        <label for="table-search" class="sr-only">Search</label>
        <div class="flex justify-center items-center">
          <p
            class="
              text-2xl text-teal-800
              font-normal
              leading-normal
              mt-0
              mb-2
              mx-2
            "
          >
            <i class="fa-solid fa-virus-covid"></i>
          </p>
          <h1
            class="
              text-4xl text-teal-800
              font-normal
              leading-normal
              mt-0
              mb-2
              underline
              decoration-violet-600
            "
          >
            Covid 19 Statistics
          </h1>
        </div>
        <div class="relative mt-1">
          <div
            class="
              absolute
              inset-y-0
              left-0
              flex
              items-center
              pl-3
              pointer-events-none
            "
          >
            <svg
              class="w-5 h-5 text-gray-400"
              fill="currentColor"
              viewBox="0 0 20 20"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
                clip-rule="evenodd"
              ></path>
            </svg>
          </div>
          <input
            type="text"
            id="table-search"
            class="
              border
              text-sm
              rounded-lg
              block
              w-80
              pl-10
              p-2.5
              bg-gray-700
              border-gray-600
              placeholder-gray-400
              text-white
              focus:ring-blue-500 focus:border-blue-500
            "
            placeholder="Search a country"
            v-model="textSearch"
            @keyup="searchCountry()"
            autofocus
          />
        </div>
      </div>
      <table class="w-full text-sm text-left text-gray-400 md:overflow-auto">
        <thead
          class="text-xs uppercase bg-gray-700 text-gray-400 md:sticky top-0"
        >
          <th class="px-6 py-3">#</th>
          <th class="px-6 py-3">Flag</th>
          <th class="px-6 py-3">Country</th>
          <th class="px-6 py-3">Continent</th>
          <th class="px-6 py-3">new cases</th>
          <th class="px-6 py-3">active cases</th>
          <th class="px-6 py-3">critical cases</th>
          <th class="px-6 py-3">Total</th>
          <th class="px-6 py-3">total death</th>
        </thead>
        <tbody v-for="(data, index) in filterDataResponse" :key="data">
          <tr class="border-b bg-gray-800 border-gray-700 hover:bg-gray-600">
            <td class="px-6 py-4">{{ index + 1 }}</td>
            <td class="px-6 py-4">
              <img
                :src="
                  'https://countryflagsapi.com/png/' +
                  data.country.split('-').join(' ')
                "
                alt=""
                class="w-20"
              />
            </td>
            <td class="px-6 py-4 font-medium text-white whitespace-nowrap">
              {{ data.country }}
            </td>
            <td class="px-6 py-4">{{ data.continent }}</td>
            <td
              :class="[
                data.cases.new > 0
                  ? 'px-6 py-6 text-red-500'
                  : 'px-6 py-6 text-green-500',
              ]"
            >
              {{ data.cases.new ? data.cases.new : 0 }}
            </td>
            <td
              :class="[
                data.cases.active > 0
                  ? 'px-6 py-6 text-red-500'
                  : 'px-6 py-6 text-green-500',
              ]"
            >
              {{ data.cases.active ? data.cases.active : 0 }}
            </td>
            <td
              :class="[
                data.cases.critical > 0
                  ? 'px-6 py-6 text-red-500'
                  : 'px-6 py-6 text-green-500',
              ]"
            >
              {{ data.cases.critical ? data.cases.critical : 0 }}
            </td>
            <td
              :class="[
                data.cases.total > 0
                  ? 'px-6 py-6 text-red-500'
                  : 'px-6 py-6 text-green-500',
              ]"
            >
              {{ data.cases.total.toLocaleString() }}
            </td>
            <td
              :class="[
                data.deaths.total > 0
                  ? 'px-6 py-6 text-red-500'
                  : 'px-6 py-6 text-green-500',
              ]"
            >
              {{ data.deaths.total ? data.deaths.total.toLocaleString() : 0 }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";


const { VUE_APP_URL, VUE_APP_API_KEY, VUE_APP_API_HOST } = process.env;

export default {
  name: "MenuIndex",
  data() {
    return {
      dataResponse: [],
      filterDataResponse: [],
      textSearch: "",
    };
  },
  async mounted() {
    const data = await axios({
      url: VUE_APP_URL,
      method: "GET",
      headers: {
        "x-rapidapi-host": VUE_APP_API_HOST,
        "x-rapidapi-key": VUE_APP_API_KEY,
      },
    });

    this.dataResponse = data.data.response.sort((a, b) =>
      a.country.localeCompare(b.country)
    );

    this.filterDataResponse = data.data.response.sort((a, b) =>
      a.country.localeCompare(b.country)
    );

    console.log(process.env)
  },
  methods: {
    searchCountry() {
      this.filterDataResponse = this.dataResponse.filter((value) =>
        value.country.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style scoped>
@media only screen and (max-width: 760px),
  (min-device-width: 768px) and (max-device-width: 1024px) {
  /* Force table to not be like tables anymore */
  table,
  thead,
  tbody,
  th,
  td,
  tr {
    display: block;
  }

  /* Hide table headers (but not display: none;, for accessibility) */
  thead tr {
    position: absolute;
    top: -9999px;
    left: -9999px;
  }

  thead {
    display: none;
  }

  tr {
    border: 1px solid #ccc;
    margin: 10px;
  }

  td {
    /* Behave  like a "row" */
    border: none;
    border-bottom: 1px solid #eee;
    position: relative;
    padding-left: 50%;
  }

  td:before {
    /* Now like a table header */
    position: absolute;
    /* Top/left values mimic padding */
    top: 6px;
    left: 6px;
    width: 45%;
    padding-right: 10px;
    white-space: nowrap;
  }

  td:nth-of-type(1):before {
    content: "#";
  }
  td:nth-of-type(2):before {
    content: "Flag";
  }
  td:nth-of-type(3):before {
    content: "Country";
  }
  td:nth-of-type(4):before {
    content: "Continent";
  }
  td:nth-of-type(5):before {
    content: "New cases";
  }
  td:nth-of-type(6):before {
    content: "Active cases";
  }
  td:nth-of-type(7):before {
    content: "Critical cases";
  }
  td:nth-of-type(8):before {
    content: "Total cases";
  }
  td:nth-of-type(9):before {
    content: "Total death";
  }
}
</style>