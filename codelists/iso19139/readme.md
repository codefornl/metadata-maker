# Omgezet vanuit xml

De codelijsten zijn beschikbaar op https://standards.iso.org/ittf/PubliclyAvailableStandards/ISO_19139_Schemas/resources/codelist/gmxCodelists.xml
Vertalingen zijn beschikbaar op https://github.com/geonetwork/core-geonetwork/blob/master/schemas/iso19139/src/main/plugin/iso19139/loc/dut/codelists.xml (GPL v2)

```javascript
//by @nanang-el-sutra: https://stackoverflow.com/questions/6453269/jquery-select-element-by-xpath
function _x(STR,D) { 
    var xresult = document.evaluate(STR, D, null, XPathResult.ANY_TYPE, null);
    var xnodes = [];
    var xres;
    while (xres = xresult.iterateNext()) {
        xnodes.push(xres);
    }
    return xnodes;
}

var xr;

//open translations
$.ajax({
  type: 'GET',
  url: "codelists.xml",
  dataType: "xml",
  success: function (xml){
    xr=xml;
  }
});

//open codelists
$.ajax({
  type: 'GET',
  url: "gmxCodelists.xml",
  dataType: "xml",
  success: function (xml){
    var test = $(xml).find('CodeListDictionary').each(function(index){
    var base = "http://schemas.opengis.net/iso/19139/20070417/resources/codelist/gmxCodelists.xml#";
    var vocab = $(this).attr("gml\:id");

    //header
    document.write('&lt;?xml version="1.0" encoding="UTF-8"?><br/>');
    document.write('&lt;rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">');
    document.write('&lt;skos:ConceptScheme xmlns:skos="http://www.w3.org/2004/02/skos/core#" rdf:about="'+base+vocab+'"><br/>');
    document.write('&lt;dc:title xmlns:dc="http://purl.org/dc/elements/1.1/" xml:lang="en">'+vocab+'&lt;/dc:title><br/>');
    document.write('&lt;/skos:ConceptScheme><br/>');

    //loop over elements
    $(this).find('codeEntry').each(function(index){
        var keyword=$(this).find('gml\\:identifier').text();
        var desc=$(this).find('gml\\:description').text();
        var mytr=$(_x('//entry[code="'+keyword+'"]',xr));

	    document.write('&lt;skos:Concept xmlns:skos="http://www.w3.org/2004/02/skos/core#" rdf:about="'+base+vocab+':'+keyword+'"><br/>');
	    document.write('&lt;skos:prefLabel xml:lang="en">'+keyword+'&lt;/skos:prefLabel><br/>');
	    document.write('&lt;skos:scopeNote xml:lang="en">'+desc+'&lt;/skos:scopeNote><br/>');
	    document.write('&lt;skos:prefLabel xml:lang="nl">'+mytr.find("label").text()+'&lt;/skos:prefLabel><br/>');
	    document.write('&lt;skos:scopeNote xml:lang="nl">'+mytr.find("description").text()+'&lt;/skos:scopeNote><br/>');
	    document.write('&lt;skos:inScheme rdf:resource="'+base+vocab+'" /><br/>&lt;/skos:Concept><br/>');

      });
      document.write('&lt;/rdf:RDF><br/><br/><br/>');
    });
  }
});
```




