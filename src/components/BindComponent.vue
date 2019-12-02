<template>
	<div>
		<div class="row">
			<div class="input-field col s10 offset-s1 m6 offset-m3">
				<textarea  id="sql" class="materialize-textarea" v-model="sql" :min-height="30" ref="sql"></textarea>
				<label for="sql">Sql</label>
			</div>
		</div>

		<div class="row">
			<div class="input-field col s10 offset-s1 m6 offset-m3">
				<textarea  id="param" class="materialize-textarea" v-model="parametros" :min-height="30" ref="param"> </textarea>
				<label for="param">Parametros</label>
			</div>
		</div>

		<div class="row">
			<div class="col s10 offset-s1 m6 offset-m3 center-align">
				<a class="waves-effect waves-light btn" v-on:click="convert(parametros)">Inserir parametros</a>
			</div>
		</div>

		<div class="row">
			<div id="resultDiv" class="input-field col s10 offset-s1 m6 offset-m3">
				<textarea  id="result" class="materialize-textarea" v-model="resultado" :min-height="30" ref="result"> </textarea>
				<label for="result">Resultado</label>
			</div>
		</div>
	</div>	
</template>

<script>
	import "materialize-css/dist/css/materialize.css"
	import "font-awesome/css/font-awesome.css"
	import M from 'materialize-css/dist/js/materialize'

	export default {
		name: 'BindComponent',
		data(){
			return { sql:"",
			parametros:"",
			resultado:""
			};
		},
		methods: {
			isNullOrEmptyOrUndefined: function(element){
				if(element != null && element != "" && element != undefined)
					return false;
				else
					return true;
			},
			formatParam: function(params){
				var paramsString = "{";
				if(params.search("DataMap")!= -1){
					let body = params.split("{")[1].split("}")[0];
					let lines = body.split("\n");
					lines = lines.map((item)=>{ return  item.trim(); });
					lines = lines.filter((item)=>{ return  !this.isNullOrEmptyOrUndefined(item); });
					lines.forEach((element, index)=>{
						if( !this.isNullOrEmptyOrUndefined(element) ){
							let key = element.split(":",1)[0]
							let value = element.split(key)[1].split(": ")[1]
							key = key.split("- ")[1]
							let lastIndex = key.lastIndexOf(' (')
							key = key.split(" (",lastIndex)[0]
							paramsString += key + "=" + value							
							if(!this.isNullOrEmptyOrUndefined(lines[index+1]))
								paramsString += ","
						}
					});
					paramsString += "}";
					if(paramsString != "{}")
						this.parametros = paramsString;
				}
			},
			convertParamsStringToVetor: function(striOri){
				var striNovo = striOri.replace(/\n/g, '');
				striNovo = striNovo.replace(/{/g, '');
				striNovo= striNovo.replace(/}/g, '');
				var v = striNovo.split(',');
				var b = [];
				b = v.map(function(entry) {
						var t = entry.split('=');
						t[0] = t[0].replace(/ /g, '');
						t[0] = this.funcOrganizaChave(t[0]);
						t[1] = this.funcOrganizaValor(t[1]);
						return t;
					});
				return b;
			},
			convert: function(params){
				this.formatParam(params);
				let retorno = this.convertParamsStringToVetor(this.parametros);
				console.log(retorno);				
			},
			funcOrganizaChave: function (texto){
				return `:${texto}`;
			},		
			funcOrganizaValor: function(texto){
				if(texto != "null")
					return `'${texto}'`;
				else
					return texto;
			}
		},
		created(){
			M.textareaAutoResize();
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

<!--

	DataMap {
	- ano (Short): 2019
	- periodo (Byte): 2
	- aluno (String): 01201920744
	- curso (String): AD14
	}

-->
