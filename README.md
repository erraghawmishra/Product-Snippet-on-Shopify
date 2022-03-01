# Product-Snippet-on-Shopify

<script type="application/ld+json">
    {
      "@context": "https://schema.org/",
      "@type": "Product",
      "name": "{{ product.title }}",
      "image": "https:{{ product.featured_image.src | img_url: "master" }}",
      "description": "{{ page_description}}",
      "sku": "{{ product.selected_or_first_available_variant.sku }}",
      "mpn": "{{ product.selected_or_first_available_variant.id }}",
      "brand": {
        "@type": "Brand",
        "name": "{{ product.selected_or_first_available_variant.vendor }}"
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
          "name": "{{ shop.name }}"
        }
      },
      "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4.4",
        "reviewCount": "89"
      },
      "offers": {
        "@type": "Offer",
        "url": "{{ product.url }}",
        "priceCurrency":"{{ shop.currency }}",
"price":"{{ product.price }}",
        "priceValidUntil": " ",
        "itemCondition": "https://schema.org/UsedCondition",
        "availability": "https://schema.org/InStock"
      }
    }
    </script>
