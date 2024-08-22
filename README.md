{
  "type": "object",
  "properties": {
    "city": {
      "type": "string"
    }
  }
}

@{body('Parse_JSON')?['city']}
