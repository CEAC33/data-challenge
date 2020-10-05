# Data Challenge

## How to run the project

```
sudo ./setup.sh 
```

## Test sending 9 photos with the api, saved in mongo and indexed in elasticsearch

### Check that there are no photos in the db

```
docker exec -it kafka-streams-playground_mongo1_1 bash
mongo
use kafka-stream-playground
db.photo.count() 
```

There should be 0 photos

### Check that there are no photos in elasticsearch

```
curl -X GET \
  http://localhost:9200/photo/_search \
  -H 'Content-Type: application/json' \
  -d '{
    "query": {
        "match_all": {}
    }
}'
```

You should see that there are 0 hits:

```
"hits":{"total":{"value":0,
```

### Send the photos to the api in a new terminal windows

```
./send-photos-to-api.sh
```

### Return to the mongo terminal an see the photos

```
db.photo.count() 
```

Now there should be 9 photos

```
db.photo.find({})
```

### See the 9 photos in elasticsearch

```
curl -X GET \
  http://localhost:9200/photo/_search \
  -H 'Content-Type: application/json' \
  -d '{
    "query": {
        "match_all": {}
    }
}'
```

You should see that there are 9 hits:

```
"hits":{"total":{"value":9,
```

## Test insertion directly to mongo

### Manually inserting in mongo

Return to the mongo terminal

```
db.photo.insert(
    { "_id" : "MANUALLYINSERTED", "color" : "#6E634A", "created_at" : "2016-05-03T15:00:28Z", "current_user_collections" : [ { "curated" : false, "id" : 206, "published_at" : "2016-01-12T23:16:09Z", "title" : "Makers:CatandBen", "updated_at" : "2016-07-10T16:00:01Z" } ], "description" : "Amandrinkingacoffee.", "downloads" : 1345, "exif" : { "aperture" : "4.970854", "exposure_time" : "2.011111111111111112", "focal_length" : "37", "iso" : 100, "make" : "Canon", "model" : "CanonEOS40D" }, "height" : 3264, "id" : "MANUALLYINSERTED", "liked_by_user" : false, "likes" : 24, "links" : { "download" : "https://unsplash.com/photos/Dwu85P9SOIk/download", "download_location" : "https://api.unsplash.com/photos/Dwu85P9SOIk/download", "html" : "https://unsplash.com/photos/Dwu85P9SOIk", "self" : "https://api.unsplash.com/photos/Dwu85P9SOIk" }, "location" : { "city" : "Montreal", "country" : "Canada", "position" : { "latitude" : 45.4732984, "longitude" : -73.6384879 } }, "tags" : [ { "title" : "man" }, { "title" : "drinking" }, { "title" : "coffee" } ], "updated_at" : "2016-07-10T16:00:01Z", "urls" : { "thumb" : "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d?q=75&fm=jpg&w=200&fit=max", "full" : "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d?q=75&fm=jpg", "regular" : "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d?q=75&fm=jpg&w=1080&fit=max", "small" : "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d?q=75&fm=jpg&w=400&fit=max", "raw" : "https://images.unsplash.com/photo-1417325384643-aac51acc9e5d" }, "user" : { "bio" : "JustaneverydayJoe", "id" : "QPxL2MGqfrw", "links" : { "portfolio" : "https://api.unsplash.com/users/exampleuser/portfolio", "self" : "https://api.unsplash.com/users/exampleuser", "likes" : "https://api.unsplash.com/users/exampleuser/likes", "html" : "https://unsplash.com/exampleuser", "photos" : "https://api.unsplash.com/users/exampleuser/photos" }, "location" : "Montreal", "portfolio_url" : "https://example.com/", "total_collections" : 13, "total_likes" : 5, "total_photos" : 10, "updated_at" : "2016-07-10T16:00:01Z", "username" : "exampleuser" }, "width" : 2448 }
)
```

There should be now 10 photos:

```
db.photo.count()
```

### Checking elasticsearch

```
curl -X GET \
  http://localhost:9200/photo/_search \
  -H 'Content-Type: application/json' \
  -d '{
    "query": {
        "match_all": {}
    }
}'
```

You should see that there are 10 hits:

```
"hits":{"total":{"value":10,
```

## Run tests

```
sbt test
```