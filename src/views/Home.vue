<template>
  <div id="container">
    <div id="kigo-list">
    	<ul>
    		<li 
    			v-for="kigo in filteredKigo"
    			@mouseover="activeKigo = kigo"
			>
    			【{{kigo.japanese}}】{{kigo.description}}
    		</li>
    	</ul>
    </div>
    <div id="active-kigo">
    	<div id="card">
	    	<span id="kanji">{{activeKigo.japanese}}</span>
	    	<div id="pronunciation">
	    		<span id="hiragana">{{activeKigo.hiragana}}</span>
	    		<span id="romanji">[{{activeKigo.pronunciation}}]</span>
	    	</div>
	    	<div>
	    		<span>{{activeKigo.description}}</span>
	    	</div>
	    	<div id="meta">
		    	<div id="categories">
		    		<span id="season">{{activeKigo.season}}</span>
		    		<span id="sub-season">{{activeKigo.subSeason}}</span>
		    		<span id="category" @click="filterKigo({category: activeKigo.category})">{{activeKigo.category}}</span>
		    	</div>
		    	<div>
		    		<!-- <span>source: {{activeKigo.source}}</span> -->
		    		source: <a>UVA Japanese eText Intiative</a>
		    	</div>
		    </div>
	    </div>
    </div>
    <div id="kigo-filters">
    	<span>Find Kigo</span>
    	<div class="filter-group">
    		<span class="filter-header">Saijiki Categories</span>
	    	<div class="filter-config">
	    		<label>Season</label>
	    		<treeselect 
	    			v-model="activeSeasons" 
	    			:multiple="true" 
	    			:options="kigoSeasons" 
					value-consists-of="LEAF_PRIORITY"
	    			:max-height= "200"
	    			placeholder= "select seasons"
				/>
	    	</div>
	    	<div class="filter-config">
	    		<label>Category</label>
	    		<treeselect 
	    			v-model="activeCategories" 
					:multiple="true" 
					:options="kigoCategories" 
					value-consists-of="LEAF_PRIORITY"
					:max-height= "200"
					placeholder= "click to select"
				/>
	    	</div>
	    </div>
	    <div class="filter-group">
    		<span class="filter-header">Saijiki.js Categories</span>
	    	<div class="filter-config">
	    		<label>Time Of Day</label>
	    		<treeselect 
	    			v-model="activeSeasons" 
	    			:multiple="true" 
	    			:options="kigoSeasons" 
					value-consists-of="LEAF_PRIORITY"
	    			:max-height= "200"
	    			placeholder= "select seasons"
				/>
	    	</div>
	    	<div class="filter-config">
	    		<label>Weather</label>
	    		<treeselect 
	    			v-model="activeCategories" 
					:multiple="true" 
					:options="kigoCategories" 
					value-consists-of="LEAF_PRIORITY"
					:max-height= "200"
					placeholder= "click to select"
				/>
	    	</div>
	    </div>
    </div>
  </div>
</template>

<script>
import saijiki from '../saijikijs'
import Treeselect from '@riophae/vue-treeselect'
// import the styles
import '@riophae/vue-treeselect/dist/vue-treeselect.css'

export default {
  name: 'home',
  components: { Treeselect },
  data() {
  	return {
  		activeSeasons: [],
  		activeCategories: [],
  		filteredKigo: [],
  		activeKigo: {},
  		kigoCategories: [
	  		{
	  			id: 'the season',
	  			label: 'the season'
	  		},
	  		{
	  			id: 'the heavens',
	  			label: 'the heavens'
	  		},
	  		{
	  			id: 'the earth',
	  			label: 'the earth'
	  		},
	  		{
	  			id: 'the humanity',
	  			label: 'the humanity'
	  		},
	  		{
	  			id: 'the observances',
	  			label: 'the observances'
	  		},
	  		{
	  			id: 'the animals',
	  			label: 'the animals'
	  		},
	  		{
	  			id: 'the plants',
	  			label: 'the plants'
	  		}
  		],
  		kigoSeasons: [
  			{
	  			id: 'spring',
	  			label: 'spring',  				
	  			children: [
		  			{
		  				id: 'early spring',
		  				label: 'early spring'
		  			},
		  			{
		  				id: 'mid spring',
		  				label: 'mid spring'
		  			},
		  			{
		  				id: 'late spring',
		  				label: 'late spring'
		  			},
		  			{
		  				id: 'all spring',
		  				label: 'all spring'
		  			}
	  			]
  			},
  			{
	  			id: 'summer',
	  			label: 'summer',  				
	  			children: [
		  			{
		  				id: 'early summer',
		  				label: 'early summer'
		  			},
		  			{
		  				id: 'mid summer',
		  				label: 'mid summer'
		  			},
		  			{
		  				id: 'late summer',
		  				label: 'late summer'
		  			},
		  			{
		  				id: 'all summer',
		  				label: 'all summer'
		  			}
	  			]
  			},
  			{
	  			id: 'autumn',
	  			label: 'autumn',  				
	  			children: [
		  			{
		  				id: 'early autumn',
		  				label: 'early autumn'
		  			},
		  			{
		  				id: 'mid autumn',
		  				label: 'mid autumn'
		  			},
		  			{
		  				id: 'late autumn',
		  				label: 'late autumn'
		  			},
		  			{
		  				id: 'all autumn',
		  				label: 'all autumn'
		  			}
	  			]
  			},
  			{
	  			id: 'winter',
	  			label: 'winter',  				
	  			children: [
		  			{
		  				id: 'early winter',
		  				label: 'early winter'
		  			},
		  			{
		  				id: 'mid winter',
		  				label: 'mid winter'
		  			},
		  			{
		  				id: 'late winter',
		  				label: 'late winter'
		  			},
		  			{
		  				id: 'all winter',
		  				label: 'all winter'
		  			}
	  			]
  			},
  			{
  				id: 'New Year',
  				label: 'New Year'
  			}
  		]
  	}
  },
  computed: {
  	today: function() {
  		return saijiki.today(true)
  	},
  	filterParams: function() {
  		let filter = {}

  		if(this.activeSeasons.length > 0) filter.subSeason = this.activeSeasons
  		if(this.activeCategories.length > 0) filter.category = this.activeCategories

  		return filter
  	}
  },
  watch: {
  	filterParams: function(val) {
  		console.log(val)
  		this.filterKigo(val)
  	}
  },
  methods: {
  	filterKigo: function(filterObject) {
  		this.filteredKigo = saijiki.search(filterObject)
  	}
  },
  mounted() {
  	this.filteredKigo = saijiki.today(true)
  }

};
</script>

<style lang="scss">
#container {
	height: 600px;
	width: 980px;
	margin: 0 auto;
	display: grid;
	grid-template-columns: 1fr 1fr 1fr;
	grid-template-rows: 1fr 1fr 1fr 1fr 1fr;
	grid-column-gap: 0px;
	grid-row-gap: 0px;

	#kigo-list { 
	  grid-area: 1 / 1 / 5 / 2; 
		overflow: scroll;
	}

	#active-kigo { 
	  grid-area: 1 / 2 / 5 / 4; 

	  #card {
	  	width: 450px;
	  	margin: 0 auto;
	  	height: 100%;
	  	position: relative;

	  	#kanji {
	  		font-size: 80px;
	  	}

	  	#pronunciation {
	  		margin-bottom: 10px;
		  	#hiragana {
				font-size: 18px;
				margin-right: 8px;
		  	}

		  	#romanji {
		  		font-size: 14px;
		  	}
	  	}

	  	#meta {
	  		position: absolute;
	  		bottom: 0px;

	  		span {
	  			display: inline-block;
	  			width: 100px;
	  			padding: 2px;
	  			text-align: center;
	  		}

	  		#season {
	  			border: 1px solid white;
	  		}

	  		#sub-season {
	  			border: 1px solid grey;
	  		}

	  		#category {
	  			border: 1px solid pink;
	  		}
	  	}

	  }
	}


	.vue-treeselect__multi-value-item {
		background: none;
		color: #555;
	}

	.vue-treeselect__icon vue-treeselect__value-remove {
		color: #555
	}

	#kigo-filters { 
		grid-area: 5 / 1 / 6 / 4; 

		.filter-group {
			margin-bottom: 20px;
		}

		.filter-header {
			display: block;
			font-weight: 900;
			font-size: 18px;
			margin-bottom: 4px;
		}

		.filter-config {
			display: inline-block;
			width: 30%;
			margin-right: 8px;

		}

	}


}
</style>
