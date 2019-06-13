<template>
	<div>
	  <div id="container">
	  	<div id="kigo-meta">
	  		<span 
	  			class="metaButton" 
	  			@click="activeMeta = 'list'"
	  			:class="{active: activeMeta === 'list'}"
				>kigo</span> 
	  		<span 
	  			class="metaButton" 
	  			@click="activeMeta = 'filter'"
	  			:class="{active: activeMeta === 'filter'}"
				>filter</span>
	  	</div>
			<div id="kigo-list">
				<div v-if="activeMeta === 'list'">
		    	<ul>
		    		<li
		    			v-for="kigo in filteredKigo"
		    			@mouseover="activeKigo = kigo"
		    			:class="{active: kigo === activeKigo}"
						>
		    			<span id="japanese">{{kigo.japanese}}</span> | <span id="english">{{kigo.description}}</span>
		    		</li>
		    	</ul>
				</div>
				<div v-if="activeMeta === 'filter'">
	    	<div id="saijiki-group">
	    		<span class="filter-header">Traditional Categories</span>
		    	<div class="filter-config">
		    		<label>Season</label>
		    		<treeselect 
		    			v-model="activeSeasons" 
		    			:multiple="true" 
		    			:options="kigoSeasons" 
							value-consists-of="LEAF_PRIORITY"
		    			:max-height= "200"
		    			placeholder= "filter season"
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
						placeholder= "filter category"
					/>
		    	</div>
		    </div>
			    <div id="custom-group">
		    		<span class="filter-header">Saijiki.js Categories</span>
			    	<div class="filter-config">
			    		<label>Time Of Day</label>
			    		<treeselect 
			    			v-model="activeTimes" 
			    			:multiple="true" 
			    			:options="kigoTimes" 
								value-consists-of="LEAF_PRIORITY"
			    			:max-height= "200"
			    			placeholder= "filter time"
						/>
			    	</div>
			    	<div class="filter-config">
			    		<label>Weather</label>
			    		<treeselect 
			    			v-model="activeWeather" 
							:multiple="true" 
							:options="kigoWeather" 
							value-consists-of="LEAF_PRIORITY"
							:max-height= "200"
							placeholder= "filter weather"
						/>
			    	</div>
			    	<div class="filter-config">
			    		<label>Conditions</label>
			    		<treeselect 
		    			v-model="activeConditions" 
							:multiple="true" 
							:options="kigoConditions" 
							value-consists-of="LEAF_PRIORITY"
							:max-height= "200"
							placeholder= "filter conditions"
						/>
			    	</div>
			    </div>
				</div>
	    </div>
	    <div id="active-kigo">
	    	<div id="card">
		    	<span id="kanji">{{activeKigo.japanese}}</span>
		    	<div id="pronunciation">
		    		<span id="hiragana">{{activeKigo.hiragana}}</span>
		    		<span id="romanji">[{{activeKigo.pronunciation}}]</span>
		    	</div>
		    	<div id="meaning">
		    		<span class="label">translation</span> 
		    		<span class="text">{{activeKigo.description}}</span>
		    		<span v-if="activeKigo.commentary"><span class="label">commentary</span></span>
		    		<span class="text">{{activeKigo.commentary}}</span>
		    	</div>
		    	<div id="meta">
			    	<div id="categories">
			    		<span>Categories: </span>
			    		<span class="kigoMeta">{{activeKigo.season}}</span>
			    		<span class="kigoMeta">, {{activeKigo.subSeason}}</span>
			    		<span class="kigoMeta">, {{activeKigo.category}}</span>
			    		<span class="kigoMeta" v-if="activeKigo.timeOfDay">, {{activeKigo.timeOfDay}}</span>
			    		<span class="kigoMeta" v-if="activeKigo.weather">, {{activeKigo.weather}}</span>
			    		<span class="kigoMeta" v-if="activeKigo.generalConditions">, {{activeKigo.generalConditions}}</span>
			    	</div>
			    	<div>
			    		<span>Source: {{activeKigo.text}} <a :href="activeKigo.source">[via]</a></span>
			    	</div>
			    	<div id="external">
			    		<a :href="externalSearch" target="_blank">Find In Japanese Saijiki</a>
			    	</div>
			    </div>
		    </div>
	    </div>
		  <div id="explanation">
		  	<p>This is a demo of saijiki.js - a JavaScript saijki for computational poets and net artists.</p> 
		  	<p>You can read more about saijki and kigo here or get the repo and incorporate the library into your own work here</p>
		  </div>
	  </div>
	  <div id="links">
	  	<a href="https://under-construction.club">Under Construction Club</a> |
	  	<a href="mailto:alexcarusillo@gmail.com">Contact</a>
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
  		activeMeta: 'list',
  		activeSeasons: [],
  		activeCategories: [],
  		activeTimes: [],
  		activeWeather: [],
  		activeConditions: [],
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
  		],
  		kigoTimes: [
  			{
	  			id: 'morning',
	  			label: 'morning',  				
  			},
  			{
	  			id: 'afternoon',
	  			label: 'afternoon',  				
  			},
  			{
	  			id: 'evening',
	  			label: 'evening',  				
  			}
  		],
  		kigoWeather: [
  			{
	  			id: 'rain',
	  			label: 'rain',  				
  			},
  			{
	  			id: 'snow',
	  			label: 'snow',  				
  			},
  			{
	  			id: 'cloudy',
	  			label: 'cloudy',  				
  			},
  			{
	  			id: 'clear',
	  			label: 'clear',  				
  			}
  		],
  		kigoConditions: [
  			{
	  			id: 'hot',
	  			label: 'hot',  				
  			},
  			{
	  			id: 'cold',
	  			label: 'cold',  				
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
  		if(this.activeTimes.length > 0) filter.timeOfDay = this.activeTimes
  		if(this.activeWeather.length > 0) filter.weather = this.activeWeather
  		if(this.activeConditions.length > 0) filter.generalConditions = this.activeConditions

  		return filter
  	},
  	externalSearch: function() {
  		var queryString = 'https://www.google.com/search?q=site%3Akotobank.jp+OR+site%3Akigosai.sub.jp+OR+site%3Amysai.net+'
  		return queryString + this.activeKigo.japanese
  	}
  },
  watch: {
  	filterParams: function(val) {
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
  	this.activeKigo = this.filteredKigo[0]
  }

};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Noto+Sans+JP:100,300,400,500,700&display=swap&subset=japanese');
$active-color: #ff3d0033;
$hover-color: black;
@treeselect-font-size: 40px; 

body {
	font-family: 'Noto Sans JP', sans-serif;
}

#container {
	height: 600px;
	width: 980px;
	margin: 40px auto 0px auto;
	display: grid;
	grid-template-columns: 2fr 1fr 1fr 1fr;
	grid-template-rows: 40px 1fr 1fr 40px;
	grid-column-gap: 20px;
	grid-row-gap: 10px;

	#kigo-meta {
		grid-column: 1 / 2;
		grid-row: 1 / 2;

		.metaButton {
			border: 2px solid black;
			padding: 8px;
			margin-right: 10px;
			cursor: pointer;
	   -moz-user-select: none;
	   -khtml-user-select: none;
	   -webkit-user-select: none;
	   -ms-user-select: none;
	   user-select: none;
		}

		.metaButton:hover {
			background: #F8B893;
		}
	}

	#kigo-list { 
		grid-column: 1 / 2;
		grid-row: 2 / 4;
		overflow: scroll;
		height: 100%;

		ul {
			list-style: none;
			padding-left: 0;	
		}

		li {
			display: flex;
			height: 40px;
			border: 2px solid black;
			margin-bottom: 8px;
			padding-left: 4px;
			align-items: center;

			#japanese {
				margin-right: 10px;
			}

			#english {
				margin-left: 10px;
			}
		}

	}

	#active-kigo { 
		grid-column: 2 / 5; 
		grid-row:  1  / 4;

	  #card {
	  	padding: 0px 10px;
	  	border: 2px solid black;
	  	height: 100%;
	  	position: relative;

	  	#kanji {
	  		font-size: 80px;
	  	}

	  	#pronunciation {
	  		font-weight: 100;
	  		margin-bottom: 10px;

		  	#hiragana {
				font-size: 18px;
				margin-right: 8px;
		  	}

		  	#romanji {
		  		font-size: 14px;
		  	}
	  	}

	  	#meaning {

	  		.label {
	  			font-weight: 400;
	  			margin-bottom: 0px;
	  		}

	  		.text {
	  			font-weight: 300;
	  			margin-bottom: 12px;
	  		}

	  		span {
	  			display: block;
	  		}
	  	}

	  	#meta {
	  		position: absolute;
	  		bottom: 0px;
	  		margin: 0 auto;
	  		font-size: 14px;
	  		font-weight: 300;

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

	#filter-meta {
		grid-column: 1 / 2;
		grid-row: 3 / 4;
	}

	#explanation {
		grid-column: 1 / 5;
		grid-row: 4 / 5;
		margin-top: 40px;
		font-weight: 100;
		font-size: 14px;

		p {
			margin: 0px;
		}
	}


	.filter-header {
		display: block;
		font-size: 18px;
		font-weight: 100;
		margin: 30px 0px 5px 0px;
	}

	.filter-config {
		margin-bottom: 10px;
		display: block;
	}

	#saijiki-group {
		grid-column: 2 / 4; 
		grid-row: 3 / 4;
	}

		#custom-group {
			grid-column: 4 / 6; 
			grid-row: 3 / 4;
		}

	.active {
		background-color: $active-color;
	}

	.hover {
		background-color: $hover-color;
	}

}

#links {
	width: 100%;
	font-weight: 100;
	font-size: 14px;
	background: #f9edd6;
	padding: 10px;
	position: absolute;
	bottom: 0px;
}

.vue-treeselect {
	font-size: 11px;
}

.vue-treeselect__control {
	border: 2px solid black !important;
	border-radius: 0;
	background: none;
}

.vue-treeselect__menu {
	background: #F5DDAF;
}

.vue-treeselect__option--highlight {
  background:  #F0D091;
}

.vue-treeselect svg {

    fill: black;

}

.vue-treeselect__value-remove {
	border-left: none;
}


.vue-treeselect__multi-value-item {
	background: none;
	color: #555;
}

.vue-treeselect__icon vue-treeselect__value-remove {
	color: #555
}


</style>
