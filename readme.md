```html
<!doctype HTML>
<head>
	<script src="http://code.jquery.com/jquery-2.1.0.min.js"></script>
	<script src="js/highlight.pack.js"></script>

	<script type="text/javascript" src="//use.typekit.com/wze1wut.js?n8wtaj"></script>
	<link rel="stylesheet" href="https://www.e-conomic.com/css/w-screen.css?open&amp;v=20150319" media="screen">
	<link rel="stylesheet" href="https://www.e-conomic.com/css/facelift.css?open&amp;v=2" media="screen">
	<style type="text/css">.tk-proxima-nova,body{font-family:"proxima-nova",sans-serif;}.tk-adelle{font-family:"adelle",serif;}</style>
	<script>try{Typekit.load();}catch(e){}</script>
	<link rel="stylesheet" href="css/stylesheet.css">
	<link rel="stylesheet" href="css/highlight.styles/tomorrow.css">
	
</head>
<body>
	<div class="row pbe md_bgr_t4">
		<article class="wrap">
			<div class="cell s1of1" id="demo">
				<h1 class="hs2">REST Demo</h1>
				<span class="md_input_t1" onclick="update('')" title="Click to go to root" style="cursor: hand;cursor: pointer;">https://restapi.e-conomic.com/</span><input id="requestpath" class="md_input_w3 md_input_t1" type="text" placeholder="products/" size="20"></input><span class="md_input_t1">?demo=true</span><input id="parameters" class="md_input_w3 md_input_t1" type="text" placeholder="&filter=productNumber$lt:102" size="20"></input><button onclick="interactive_call();return false;" class="md_but_t2 md_rad2 fs8">Go</button>
			<br>
			<small>Examples? <a href="#" onclick="update('products', '&filter=productNumber$gt:112')">ProductNumbers greater than 112</a> or perhaps <a href="#" onclick="update('invoices-experimental/drafts', '&filter=date$eq:2014-11-21')">Invoices from Nov. 21st 2014</a></small>
			<br><br>
			<button class="btn-fullscreen md_but_t2 md_rad2 fs8">Toggle fullscreen</button>
			<br><br>
			<pre id="restcontainer"><code id="output" class="json">...</code></pre>
		
			<script>
			    function update(call, extension){
			        jQuery('#requestpath').val(call);
			        jQuery('#parameters').val(extension);
			        interactive_call();
			    }
			
			    function interactive_call(){
			        var content = jQuery('#requestpath').val()
			        
			        //Default content for path
			        if(content == 'banana'){
			        	jQuery('#output').text('Maybe check your phone? There\'s no banana endpoints here.');
			        	var phone = $("<iframe id='easter' type='text/html' width='940' height='600' src='http://www.youtube.com/embed/j5C6X9vOEkU?autoplay=1&start=11&controls=0&showinfo=0' frameborder='0'/>");
			            $('#restcontainer').after(phone);
			        }
			        else if(content == 'rasmus'){
			        	jQuery('#output').text('Oh Rasmus, we love thee :-)');
			            var sigs = $("<iframe id='easter' type='text/html' width='940' height='600' src='http://www.youtube.com/embed/s_B44jS0XrQ?autoplay=1&controls=0&showinfo=0' frameborder='0'/>");
			            $('#restcontainer').after(sigs);
			        }
			        
			        else {
			        //start på else
			        $('#easter').attr("src", " ");
			        $('#easter').remove();
			        var parameters = jQuery('#parameters').val()
			        var call_url = 'https://restapi.e-conomic.com/' + content + '?demo=true' + parameters;
			        jQuery.ajax({
			      dataType: 'json',
			      url: call_url,
			      context: document.body
			    }).complete(function(data) {
			        
			        if (data['status'] == 500) {
			            jQuery('#output').text(data['status'] + ' ' + data['statusText']);
			        }
			        else {
			            var d = jQuery.parseJSON(data['responseText']);
			            jQuery('#output').text(JSON.stringify(d, null, 4));
			            
			            //Run highlight.js on our code block
			            $('pre code').each(function(i, block) {
			                hljs.highlightBlock(block);
			              });
			              
			            //Find all REST self links and make them interactive by wrapping with a+onclick
			            $('.hljs-string').each( function() {
			                str = $(this).text();
			                //console.log(str + '- eached - not tested!');
			                
			                //Check if .hljs-string classed element contains starts with "https  
			                if(/("https.*\?)/i.test(str)) {
			                   //console.log(str + '- match found!');
			                   //console.log(this)
			                   //the path is everything after the REST API root URL and up until we start the URL parameters with '?'. So the leading "https://.. etc can be removed as we add it staticly.
			                   thispath = str.slice(31, str.indexOf('?'))
			                   //The parameters presume that we cut away ?demo=true as we've decided this is a static part of our REST demo. The last character being cut off is the doublequote around the JSON string.
			                   parameters = str.slice( str.indexOf("&"), -1 )
		
			                   $( this ).wrap( '<a href="#" onclick="update(' + "'" + thispath + "'" + ',' + "'" + parameters + "'" + ')"></a>' );
			                }
			             });
			            
			        }
			        
			    });
			    }
			    			 //end else 
			    			 }
			   
				$("#requestpath, #parameters").keyup(function(event) {
				    if (event.which == 13) {
				        event.preventDefault();
				        interactive_call();
				   	}
				});
				
				$(".btn-fullscreen").click(function(){
				    $("#demo").toggleClass('fullscreen');
				    $(".wrap").toggleClass('nowrap');
				  });
			</script>
			</div>
			</div>
		</article>
	</div>
</body>
```