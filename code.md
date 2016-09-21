# Geocode
google yer bulmaca

geocode <- function(address,reverse=FALSE)  {
  require("RJSONIO")
  baseURL <- "http://maps.google.com/maps/api/geocode/json?sensor=false&"
  
  # This is not necessary, 
  # because the parameter "address" accepts both formatted address and latlng
  
  conURL <- ifelse(reverse,paste0(baseURL,'latlng=',URLencode(address)),
                   paste0(baseURL,'address=',URLencode(address)))  
  con <- url(conURL)  
  data.json <- fromJSON(paste(readLines(con), collapse=""))
  close(con) 
  status <- data.json["status"]
  if(toupper(status) == "OK"){
    t(sapply(data.json$results,function(x) {
      list(address=x$formatted_address,lat=x$geometry$location[1],
           lng=x$geometry$location[2])}))
  } else { 
    warning(status)
    NULL 
  }
}

geocode("Inonu Mahallesi, Celasun Caddesi, TR")
