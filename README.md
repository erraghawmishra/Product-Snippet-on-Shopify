# Product-Snippet-on-Shopify

{% if template contains "product" %}
    <script>
      var jsonldScript = document.createElement('script');
      jsonldScript.setAttribute('type', 'application/ld+json');
      var structuredData = {
        "@context": "https://schema.org/",
        "@type": "Product",
        "name": "{{product.title}}",
         {%- if product.featured_image -%}
               "image": "https:{{ product.featured_image.src | img_url: "master" }}",
         {%- endif -%}
        "description": "{{product.description  | strip_html | json }}",
 		 {%if product.selected_or_first_available_variant.barcode %}
        	  "mpn": "{{product.selected_or_first_available_variant.barcode}}",
         {%else%}
            "mpn": "123",                        
         {% endif%}
        "brand": {
        	"@type": "Brand",
      	    "name": "{{product.vendor}}"
      },
          
       "review": {
        "@type": "Review",
        "reviewRating": {
          "@type": "Rating",
          "ratingValue": "4",
          "bestRating": "5"
        },
        "author": {
          "@type": "Person",
          "name": "Fred Benson"
        }
      },
      "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4.4",
        "reviewCount": "89"
      },
         "offers" : [
                    {%for variant in product.variants -%}{
                        "@type" : "Offer" ,
                       
                         {%if variant.barcode %}
                                  	"mpn": "{{variant.barcode}}",

                                  {%else%}
                                  	"mpn": "123",                        
                                  {% endif%}
                                  
                        "priceCurrency" : "{{ shop.currency }}" ,
                        "price" : "{{ variant.price | money_without_currency | split: "" | reverse | join: "" | remove_first: "00." | split: "" | reverse | join: "" | replace: ",", "" }}" ,
                        "availability" : "http://schema.org/{%if variant.available%}InStock{%else%}OutOfStock{%endif%}" ,
                        "itemCondition": "http://schema.org/NewCondition",
                                                                "priceValidUntil": "{{ "now" | plus: 5| date: "%Y-%m-%d %H:%M" }}",

                    {%- if variant.sku -%}
                        "sku": "{{ variant.sku | replace: '"', ''}}",
                        {%else%}
                         "sku": "456",
                    {%- endif -%}
                    {%- if variant.title != "Default Title"%}
                        "name": "{{ variant.title | strip_newlines | strip_html | escape_once | replace: '\', '\\\\' }}",
                    {%- endif%}
                        "url" : "{{ shop.url | append: variant.url }}",
                        "seller" : {
                            "@type" : "Organization",
                            "name" : "{{ shop.name | strip_newlines | strip_html | escape_once | replace: '\', '\\\\' }}"
                        }
                    }{%unless forloop.last%}, 
                    {%endunless%}{%endfor%}
                ],
           
        
        "@id": {{ canonical_url | append: '#product' | json }},
        "review": []
      };

      var widget = document.createElement("div");
      widget.innerHTML = `{{ product.metafields.judgeme.widget }}`;
      widget.querySelectorAll(".jdgm-rev").forEach(review => {
        var date = review.querySelector(".jdgm-rev__timestamp").getAttribute("data-content");
        var body = review.querySelector(".jdgm-rev__body").innerHTML.replace(/\n|<.*?>/g,'');
        var rating = review.querySelector(".jdgm-rev__rating").getAttribute("data-score");
        var authorName = review.querySelector(".jdgm-rev__author").innerText;
        var title = review.querySelector(".jdgm-rev__title").innerText;
      
        structuredData.review.push({
          "@type": "Review",
          "datePublished": date,
          "name": title,
          "reviewBody": body,
          "reviewRating": {
            "@type": "Rating",
            "ratingValue": rating,
            "bestRating": "5",
            "worstRating": "1"
          }, 
          "author": {
            "@type": "Person",
            "name": authorName
          }
        });
      });

      jsonldScript.textContent = JSON.stringify(structuredData);
      document.head.appendChild(jsonldScript);
    </script>
  {% endif %}
