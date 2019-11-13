<template>
  <div class="container">

    <div class="row">
      <div class="input-field col s6 offset-s3">
        <textarea  id="sql" class="materialize-textarea" type="text"></textarea>
        <label for="sql">Sql</label>
      </div>
    </div>

    <div class="row">
      <div class="input-field col s6 offset-s3">
        <textarea  id="param" class="materialize-textarea" type="text"> </textarea>
        <label for="param">Parametros</label>
      </div>
    </div>

    <div class="row">
      <div class="col s6 offset-s3">
        <a class="waves-effect waves-light btn" onclick="convert()">Inserir parametros</a>
      </div>
    </div>

    <div class="row">
      <div id="resultDiv" class="input-field col s6 offset-s3">
        <textarea  id="result" class="materialize-textarea" type="text"> </textarea>
        <label for="result">Resultado</label>
      </div>
    </div>

  </div>
</template>

<script>

  import "materialize-css/dist/css/materialize.css"
  import "font-awesome/css/font-awesome.css"

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  methods: {
    onReturn: function(data) { 
			$('#result').text(data['result']); 
			console.log(stri);
		},
		formatFromDataMap: function(){
			var paramsString = "{";
			var params = document.getElementById("param").value
			var inic = params.split("{")[0]
			if(inic == "DataMap " || inic == "DataMap" || inic == " DataMap "){
				var body = params.split("{")[1]
				var lines = body.split("\n");
				var line = "";
				for (var i = 0; i<lines.length;line = lines[i++]){
					if(line != null && line != "" ){
						var pair =  line.split(":")
						var key = line.split(":",1)[0]
						var value = line.split(key)[1].split(": ")[1]
						key = key.split("- ")[1]
						var lastIndex = key.lastIndexOf(' (')
						key = key.split(" (",lastIndex)[0]
						console.log(key+"-"+value)
						paramsString += key + "=" + value
						if(i+1<lines.length){
							paramsString += ","
						}
					}
				}
				paramsString+="}"
				if(paramsString != "{}"){
					document.getElementById("param").value = paramsString
					console.log(paramsString)
				}
			}
		},
		convert: function(){
			formatFromDataMap();
			var striOri = document.getElementById("param").value;
			var retorno = convertParamsStringToVetor(striOri);
			var striSql = document.getElementById("sql").value;
			var stri = striSql;
			retorno.forEach(function(each){
				stri = stri.replace(new RegExp(each[0], 'g'),each[1]) 
			});
			
			$.ajax({
				url: 'https://sqlformat.org/api/v1/format',
				type: 'POST',
				dataType: 'json',
				crossDomain: true,
				data: {sql: stri, reindent: 1},
				success: onReturn,
			});
		},
		convertParamsStringToVetor : function(striOri){
			var striNovo = striOri.replace(/\n/g, '');
			striNovo = striNovo.replace(/{/g, '');
			striNovo= striNovo.replace(/}/g, '');
			console.log(striNovo);
			var v = striNovo.split(',');
			var b = [];
			v.forEach(function(entry) {
				var t = entry.split('=');
				t[0] = t[0].replace(/ /g, '');
				t[0] = funcOrganizaChave(t[0]);
				t[1] = funcOrganizaValor(t[1]);
				b.push(t);
			});
			return b;
		},
		funcOrganizaChave: function (texto){
			return "\:".concat(texto);
		},
		funcOrganizaValor: function (texto){
			if(texto != "null")
				return "\'".concat(texto,"\'");
			else
				return texto;
		}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
