``` r
library(vegalite)

dat <- jsonlite::fromJSON('[
    {"a": "A","b": 28}, {"a": "B","b": 55}, {"a": "C","b": 43},
    {"a": "D","b": 91}, {"a": "E","b": 81}, {"a": "F","b": 53},
    {"a": "G","b": 19}, {"a": "H","b": 87}, {"a": "I","b": 52}
  ]')

static_page <- function(vl_spec, file_path){
  output_string <-  paste(
    "<!DOCTYPE html>
    <html>
    <head>
    <title>Embedding Vega-Lite</title>
    <script src=\"https://cdn.jsdelivr.net/npm/vega@3\"></script>
      <script src=\"https://cdn.jsdelivr.net/npm/vega-lite@2\"></script>
        <script src=\"https://cdn.jsdelivr.net/npm/vega-embed@3\"></script>
          </head>
          <body>
          <div id=\"vis\"></div>
            
            <script type=\"text/javascript\">
              var yourVlSpec = ", 
    vl_spec, 
    ";
    vegaEmbed('#vis', yourVlSpec);
    </script>
      </body>
      </html>",
    sep = "") 
  
  write(output_string, file_path)
 }
       
vegalite() %>%
  add_data(dat) %>%
  encode_x("a", "ordinal") %>%
  encode_y("b", "quantitative") %>%
  mark_bar() %>% 
  to_spec() %>% 
  static_page(file_path =  "/tmp/vl.html")
  
 ```
