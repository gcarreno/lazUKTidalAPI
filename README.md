# UK Tidal API

Tidal information for the United Kindom from the [Admiralty](https://www.admiralty.co.uk/) of the [UK Hydrographic Office](http://www.gov.uk/UKHO).

Developer site: [https://admiraltyapi.portal.azure-api.net](https://admiraltyapi.portal.azure-api.net/).

## API

See the [OpenAPI Definition](#open-api-definition) at the end of this file.

### Station

### Stations

### TidalEvents

## Open API definition

```json
{
  "swagger": "2.0",
  "info": {
    "title": "UK Tidal API",
    "version": "1.0"
  },
  "host": "admiraltyapi.azure-api.net",
  "basePath": "/uktidalapi",
  "schemes": [
    "https"
  ],
  "securityDefinitions": {
    "apiKeyHeader": {
      "type": "apiKey",
      "name": "Ocp-Apim-Subscription-Key",
      "in": "header"
    },
    "apiKeyQuery": {
      "type": "apiKey",
      "name": "subscription-key",
      "in": "query"
    }
  },
  "security": [
    {
      "apiKeyHeader": []
    },
    {
      "apiKeyQuery": []
    }
  ],
  "paths": {
    "/api/V1/Stations/{stationId}": {
      "get": {
        "description": "Get the Tidal Station for the provided Station ID.\r\nReturned format is a standard GeoJSON Feature.\r\nThe tidal station number is used as the GeoJSON feature Id.\r\nThe feature has a Point geometry.\r\nThe following custom properties are returned:\r\n* Id - The Tidal Station Number to be used in other API operations.\r\n* Name - The Tidal Station Name.\r\n* Country - The Country that provided the data for the Tidal Station.\r\n* Continuous Heights Available - A boolean to indicate if continuous heights are available for the Tidal Station.\r\n* Footnote - Footnote text.",
        "operationId": "Stations_GetStation",
        "summary": "Station",
        "parameters": [
          {
            "name": "stationId",
            "in": "path",
            "description": "The Tidal Station ID.",
            "required": true,
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "OK",
            "schema": {
              "$ref": "#/definitions/GeoJSON.Net.Feature.Feature"
            }
          },
          "400": {
            "description": "Bad Request"
          },
          "401": {
            "description": "Unauthorised user or incorrect subscription key"
          },
          "403": {
            "description": "Quota Exceeded"
          },
          "404": {
            "description": "Not Found"
          },
          "429": {
            "description": "Too Many Requests"
          },
          "500": {
            "description": "Internal Server Error"
          }
        },
        "produces": [
          "application/json",
          "text/json"
        ]
      }
    },
    "/api/V1/Stations": {
      "get": {
        "description": "Get a list of Tidal Stations. Optionally by Name, Station Type and Bounding Box.\r\nReturned format is a standard GeoJSON FeatureCollection.\r\nThe tidal station number is used as the GeoJSON feature Id.\r\nEach feature has a Point geometry.\r\nThe following custom properties are returned:\r\n* Id - The Tidal Station Number to be used in other API operations.\r\n* Name - The Tidal Station Name.\r\n* Country - The Country that provided the data for the Tidal Station.\r\n* Continuous Heights Available - A boolean to indicate if continuous heights are available for the Tidal Station.\r\n* Footnote - Footnote text.",
        "operationId": "Stations_GetStations",
        "summary": "Stations",
        "parameters": [
          {
            "name": "name",
            "in": "query",
            "description": "The name of the station.",
            "type": "string"
          }
        ],
        "responses": {
          "200": {
            "description": "OK",
            "schema": {
              "$ref": "#/definitions/GeoJSON.Net.Feature.FeatureCollection"
            }
          },
          "401": {
            "description": "Unauthorised user or incorrect subscription key"
          },
          "403": {
            "description": "Quota Exceeded"
          },
          "429": {
            "description": "Too Many Requests"
          },
          "500": {
            "description": "Internal Server Error"
          }
        },
        "produces": [
          "application/json",
          "text/json",
          "application/xml",
          "text/xml"
        ]
      }
    },
    "/api/V1/Stations/{stationId}/TidalEvents": {
      "get": {
        "description": "Returns high and low water events for the specified Tidal Station.",
        "operationId": "TidalEvents_GetTidalEvents",
        "summary": "TidalEvents",
        "parameters": [
          {
            "name": "stationId",
            "in": "path",
            "description": "The Station ID.",
            "required": true,
            "type": "string"
          },
          {
            "name": "duration",
            "in": "query",
            "description": "Format - int32. The number of days for which the tidal events will be returned.  If this parameter is omitted, a default value of 7 is used which represents today plus the next six days.  Valid range is 1 to 7 inclusive.  Any other values will return a bad request",
            "type": "integer"
          }
        ],
        "responses": {
          "200": {
            "description": "OK: A list of Tidal Events is returned in the response body.",
            "schema": {
              "$ref": "#/definitions/UKHO.TidalPrediction.Api.Models.ITidalEventArray"
            }
          },
          "400": {
            "description": "Bad Request: The stationId parameter is malformed or not present, or the duration parameter is outside of accepted range"
          },
          "401": {
            "description": "Unauthorised user or incorrect subscription key"
          },
          "403": {
            "description": "Quota Exceeded"
          },
          "404": {
            "description": "Not Found: The requested Station ID could not be found."
          },
          "429": {
            "description": "Too Many Requests"
          },
          "500": {
            "description": "Internal Server Error"
          }
        },
        "produces": [
          "application/json",
          "text/json",
          "application/xml",
          "text/xml"
        ]
      }
    }
  },
  "definitions": {
    "GeoJSON.Net.Feature.Feature": {
      "type": "object",
      "properties": {
        "type": {
          "enum": [
            "Point",
            "MultiPoint",
            "LineString",
            "MultiLineString",
            "Polygon",
            "MultiPolygon",
            "GeometryCollection",
            "Feature",
            "FeatureCollection"
          ],
          "type": "string",
          "readOnly": true
        },
        "id": {
          "type": "string",
          "readOnly": true
        },
        "geometry": {
          "$ref": "#/definitions/GeoJSON.Net.Geometry.IGeometryObject",
          "readOnly": true
        },
        "properties": {
          "type": "object",
          "additionalProperties": {
            "type": "object"
          },
          "readOnly": true
        },
        "bbox": {
          "type": "array",
          "items": {
            "format": "double",
            "type": "number"
          }
        },
        "crs": {
          "$ref": "#/definitions/GeoJSON.Net.CoordinateReferenceSystem.ICRSObject"
        }
      }
    },
    "GeoJSON.Net.Geometry.IGeometryObject": {
      "type": "object",
      "properties": {
        "Type": {
          "enum": [
            "Point",
            "MultiPoint",
            "LineString",
            "MultiLineString",
            "Polygon",
            "MultiPolygon",
            "GeometryCollection",
            "Feature",
            "FeatureCollection"
          ],
          "type": "string",
          "readOnly": true
        }
      }
    },
    "GeoJSON.Net.CoordinateReferenceSystem.ICRSObject": {
      "type": "object",
      "properties": {
        "Type": {
          "enum": [
            "unspecified",
            "name",
            "link"
          ],
          "type": "string",
          "readOnly": true
        }
      }
    },
    "GeoJSON.Net.Feature.FeatureCollection": {
      "type": "object",
      "properties": {
        "type": {
          "enum": [
            "Point",
            "MultiPoint",
            "LineString",
            "MultiLineString",
            "Polygon",
            "MultiPolygon",
            "GeometryCollection",
            "Feature",
            "FeatureCollection"
          ],
          "type": "string",
          "readOnly": true
        },
        "features": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/GeoJSON.Net.Feature.Feature"
          }
        },
        "bbox": {
          "type": "array",
          "items": {
            "format": "double",
            "type": "number"
          }
        },
        "crs": {
          "$ref": "#/definitions/GeoJSON.Net.CoordinateReferenceSystem.ICRSObject"
        }
      }
    },
    "UKHO.TidalPrediction.Api.Models.ITidalEvent": {
      "description": "A representation of a tide time.",
      "required": [
        "EventType"
      ],
      "type": "object",
      "properties": {
        "EventType": {
          "description": "Whether the event is for a high or low tide.",
          "enum": [
            "HighWater",
            "LowWater"
          ],
          "type": "string",
          "readOnly": true
        },
        "DateTime": {
          "format": "date-time",
          "description": "The time of the tidal event. This may be missing if it is invalid",
          "type": "string",
          "readOnly": true
        },
        "Height": {
          "format": "double",
          "description": "Tidal height in metres. This may be missing if it is invalid",
          "type": "number",
          "readOnly": true
        },
        "IsApproximateTime": {
          "description": "If the time is approximate.",
          "type": "boolean",
          "readOnly": true
        },
        "IsApproximateHeight": {
          "description": "If the height is approximate.",
          "type": "boolean",
          "readOnly": true
        },
        "Filtered": {
          "description": "When set, this indicates that the tidal events either side of this tidal event have been removed.",
          "type": "boolean",
          "readOnly": true
        }
      }
    },
    "UKHO.TidalPrediction.Api.Models.ITidalEventArray": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/UKHO.TidalPrediction.Api.Models.ITidalEvent"
      }
    }
  },
  "tags": []
}
```
