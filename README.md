# Elastic Search

## Installation
1. https://www.elastic.co/downloads/elasticsearch
2. https://www.elastic.co/downloads/kibana

## Tutorials
1. ElasticSearch - https://www.elastic.co/webinars/getting-started-elasticsearch?elektra=home&storm=sub1
2. Kibana - https://www.elastic.co/webinars/getting-started-kibana?elektra=home&storm=sub2

## Running Locally
1. Launch the ElasticSearch service
```
cd C:\Program Files\Elastic\Elasticsearch\6.5.2\bin\
elasticsearch
```
2. ElasticSearch - http://localhost:9200
3. Open command prompt as administrator and launch Kibana
```
cd C:\Program Files\Elastic\Kibana\6.2.4\bin\
kibana.bat
```
4. Kibana - http://localhost:5601

## Basic Auth
1. Open C:\ProgramData\Elastic\Elasticsearch\config\elasticsearch.yml
```
xpack.security.enabled: true
```
2. Restart ElasticSearch
```
cd C:\Program Files\Elastic\Elasticsearch\6.5.2\bin
elasticsearch
```
3. Run the following command
```
cd C:\Program Files\Elastic\Elasticsearch\6.5.2\bin
elasticsearch-setup-passwords interactive
```

## Update Document
```
PUT /inspections/_doc/12345
{
  "business_address": "660 Sacramento St",
  "business_city": "San Francisco",
  "business_id": "2228",
  "business_latitude": "37.793698",
  "business_location": {
    "type": "Point",
    "coordinates": [
      -122.403984,
      37.793698
    ]
  },
  "business_longitude": "-122.403984",
  "business_name": "Tokyo Express",
  "business_postal_code": "94111",
  "business_state": "CA",
  "inspection_date": "2016-02-04T00:00:00.000",
  "inspection_id": "2228_20160204",
  "inspection_type": "Routine",
  "inspection_score":96,
  "risk_category": "Low Risk",
  "violation_description": "Unclean nonfood contact surfaces",
  "violation_id": "2228_20160204_103142"
}
```

## Partial Update Document
```
PUT /inspections/_doc/12345/_update
{
 "doc" : {
      "flagged" : true,
      "views": 0
   }
}
```

## Delete Document
```
DELETE /inspections/_doc/12345
```

## Bulk Update Document
```
POST /inspections/_doc/_bulk
{ "index": { "_id": 1 }}
{"business_address":"315 California St","business_city":"San Francisco","business_id":"24936","business_latitude":"37.793199","business_location":{"type":"Point","coordinates":[-122.400152,37.793199]},"business_longitude":"-122.400152","business_name":"San Francisco Soup Company","business_postal_code":"94104","business_state":"CA","inspection_date":"2016-06-09T00:00:00.000","inspection_id":"24936_20160609","inspection_score":77,"inspection_type":"Routine - Unscheduled","risk_category":"Low Risk","violation_description":"Improper food labeling or menu misrepresentation","violation_id":"24936_20160609_103141"}
{ "index": { "_id": 2 }}
{"business_address":"10 Mason St","business_city":"San Francisco","business_id":"60354","business_latitude":"37.783527","business_location":{"type":"Point","coordinates":[-122.409061,37.783527]},"business_longitude":"-122.409061","business_name":"Soup Unlimited","business_postal_code":"94102","business_state":"CA","inspection_date":"2016-11-23T00:00:00.000","inspection_id":"60354_20161123","inspection_type":"Routine", "inspection_score": 95}
{ "index": { "_id": 3 }}
{"business_address":"2872 24th St","business_city":"San Francisco","business_id":"1797","business_latitude":"37.752807","business_location":{"type":"Point","coordinates":[-122.409752,37.752807]},"business_longitude":"-122.409752","business_name":"TIO CHILOS GRILL","business_postal_code":"94110","business_state":"CA","inspection_date":"2016-07-05T00:00:00.000","inspection_id":"1797_20160705","inspection_score":90,"inspection_type":"Routine - Unscheduled","risk_category":"Low Risk","violation_description":"Unclean nonfood contact surfaces","violation_id":"1797_20160705_103142"}
{ "index": { "_id": 4 }}
{"business_address":"1661 Tennessee St Suite 3B","business_city":"San Francisco Whard Restaurant","business_id":"66198","business_latitude":"37.75072","business_location":{"type":"Point","coordinates":[-122.388478,37.75072]},"business_longitude":"-122.388478","business_name":"San Francisco Restaurant","business_postal_code":"94107","business_state":"CA","inspection_date":"2016-05-27T00:00:00.000","inspection_id":"66198_20160527","inspection_type":"Routine","inspection_score":56 }
{ "index": { "_id": 5 }}
{"business_address":"2162 24th Ave","business_city":"San Francisco","business_id":"5794","business_latitude":"37.747228","business_location":{"type":"Point","coordinates":[-122.481299,37.747228]},"business_longitude":"-122.481299","business_name":"Soup House","business_phone_number":"+14155752700","business_postal_code":"94116","business_state":"CA","inspection_date":"2016-09-07T00:00:00.000","inspection_id":"5794_20160907","inspection_score":96,"inspection_type":"Routine - Unscheduled","risk_category":"Low Risk","violation_description":"Unapproved or unmaintained equipment or utensils","violation_id":"5794_20160907_103144"}
{ "index": { "_id": 6 }}
{"business_address":"2162 24th Ave","business_city":"San Francisco","business_id":"5794","business_latitude":"37.747228","business_location":{"type":"Point","coordinates":[-122.481299,37.747228]},"business_longitude":"-122.481299","business_name":"Soup-or-Salad","business_phone_number":"+14155752700","business_postal_code":"94116","business_state":"CA","inspection_date":"2016-09-07T00:00:00.000","inspection_id":"5794_20160907","inspection_score":96,"inspection_type":"Routine - Unscheduled","risk_category":"Low Risk","violation_description":"Unapproved or unmaintained equipment or utensils","violation_id":"5794_20160907_103144"}
```

## Delete Index
```
DELETE /inspections
```

## Create Index
```
PUT /inspections
{
  "settings": {
    "index.number_of_shards": 1,
    "index.number_of_replicas": 0
  }
}
```

## Search Word
```
GET /inspections/_doc/_search
{
  "query": {
    "match": {
      "business_name": "soup"
    }
  }
}
```

## Search Phrase
```
GET /inspections/_doc/_search
{
  "query": {
    "match_phrase": {
      "business_name": "san francisco"
    }
  }
}
```

## And Search Condition
```
GET /inspections/_doc/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "business_name": "soup"
          }
        },
        {
          "match_phrase": {
            "business_name": "san francisco"
          }
        }
      ]
    }
  }
}
```
```
GET /inspections/_doc/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "business_name": "soup"
          }
        }
      ]
    }
  }
}
```

## Greater than and Order By
```
GET /inspections/_doc/_search
{
  "query": {
      "range": {
        "inspection_score": {
          "gte": 80
        }
      }
  },
  "sort": [
    { "inspection_score" : "desc" }
  ]
}
```

## SQL Format Search
```
POST /_xpack/sql?format=txt
{
    "query": "SELECT business_name, inspection_score FROM inspections ORDER BY inspection_score DESC LIMIT 5"
}
```

## Aggregations
```
GET /inspections/_doc/_search
{
  "query": {
    "match": {
      "business_name": "soup"
    }
  }
 ,"aggregations" : {
    "inspection_score" : {
      "range" : {
        "field" : "inspection_score",
        "ranges" : [
          {
            "key" : "0-80",
            "from" : 0,
            "to" : 80
          },
          {
            "key" : "81-90",
            "from" : 81,
            "to" : 90
          },
          {
            "key" : "91-100",
            "from" : 91,
            "to" : 100
          }
        ]
      }
    }
  }
}
```

## Geo Search Mapping = Schema
```
DELETE inspections
PUT /inspections
GET inspections/_mapping/_doc
PUT inspections/_mapping/_doc
{
      "properties": {
          "business_address": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "business_city": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "business_id": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "business_latitude": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "coordinates": {
              "type": "geo_point"
          },
          "business_longitude": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "business_name": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "business_phone_number": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "business_postal_code": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "business_state": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "inspection_date": {
            "type": "date"
          },
          "inspection_id": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "inspection_score": {
            "type": "long"
          },
          "inspection_type": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "risk_category": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "violation_description": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "violation_id": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }
        }
}
```

## Geo Search
```
GET /inspections/_doc/_search
{
  "query": {
    "match": { "business_name": "soup"}
  },
  "sort": [
    {
      "_geo_distance": {
        "coordinates": {
          "lat":  37.783527,
          "lon": -122.409061
        },
        "order":         "asc",
        "unit":          "km"
      }
    }
    ]
}
```

## Analyze
```
GET /inspections/_analyze
{
  "tokenizer": "standard",
  "text": "my email address test123@company.com"
}

GET /inspections/_analyze
{
  "tokenizer": "whitespace",
  "text": "my email address test123@company.com"
}

GET /inspections/_analyze
{
  "tokenizer": "standard",
  "text": "Brown fox brown dog"
}

# And filters manipulate those tokens

GET /inspections/_analyze
{
  "tokenizer": "standard",
  "filter": ["lowercase"],
  "text": "Brown fox brown dog"
}

GET /inspections/_analyze
{
  "tokenizer": "standard",
  "filter": ["lowercase", "unique"],
  "text": "Brown brown brown fox brown dog"
}
```