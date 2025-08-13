```bash
GET _index_template
```


{
  "index_templates" : [
    {
      "name" : "synthetics",
      "index_template" : {
        "index_patterns" : [
          "synthetics-*-*"
        ],
        "composed_of" : [
          "synthetics-mappings",
          "data-streams-mappings",
          "synthetics-settings"
        ],
        "priority" : 100,
        "version" : 1,
        "_meta" : {
          "managed" : true,
          "description" : "default synthetics template installed by x-pack"
        },
        "data_stream" : {
          "hidden" : false
        },
        "allow_auto_create" : true
      }
    },
    {
      "name" : ".watch-history-13",
      "index_template" : {
        "index_patterns" : [
          ".watcher-history-13*"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "format" : "6",
              "lifecycle" : {
                "name" : "watch-history-ilm-policy"
              },
              "hidden" : "true",
              "number_of_shards" : "1",
              "auto_expand_replicas" : "0-1",
              "number_of_replicas" : "0"
            }
          },
          "mappings" : {
            "_meta" : {
              "watcher-history-version" : "13"
            },
            "dynamic" : false,
            "dynamic_templates" : [
              {
                "disabled_payload_fields" : {
                  "match_pattern" : "regex",
                  "path_match" : """result\.(input(\..+)*|(transform(\..+)*)|(actions\.transform(\..+)*))\.payload""",
                  "mapping" : {
                    "type" : "object",
                    "enabled" : false
                  }
                }
              },
              {
                "disabled_search_request_body_fields" : {
                  "match_pattern" : "regex",
                  "path_match" : """result\.(input(\..+)*|(transform(\..+)*)|(actions\.transform(\..+)*))\.search\.request\.(body|template)""",
                  "mapping" : {
                    "type" : "object",
                    "enabled" : false
                  }
                }
              },
              {
                "disabled_exception_fields" : {
                  "match_pattern" : "regex",
                  "path_match" : """result\.(input(\..+)*|(transform(\..+)*)|(actions\.transform(\..+)*)|actions)\.error""",
                  "mapping" : {
                    "type" : "object",
                    "enabled" : false
                  }
                }
              },
              {
                "disabled_jira_custom_fields" : {
                  "path_match" : "result.actions.jira.fields.customfield_*",
                  "mapping" : {
                    "type" : "object",
                    "enabled" : false
                  }
                }
              }
            ],
            "properties" : {
              "exception" : {
                "type" : "object",
                "enabled" : false
              },
              "metadata" : {
                "dynamic" : true,
                "type" : "object"
              },
              "trigger_event" : {
                "dynamic" : true,
                "type" : "object",
                "properties" : {
                  "schedule" : {
                    "dynamic" : true,
                    "type" : "object",
                    "properties" : {
                      "scheduled_time" : {
                        "type" : "date"
                      }
                    }
                  },
                  "triggered_time" : {
                    "type" : "date"
                  },
                  "type" : {
                    "type" : "keyword"
                  },
                  "manual" : {
                    "dynamic" : true,
                    "type" : "object",
                    "properties" : {
                      "schedule" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "scheduled_time" : {
                            "type" : "date"
                          }
                        }
                      }
                    }
                  }
                }
              },
              "result" : {
                "dynamic" : true,
                "type" : "object",
                "properties" : {
                  "input" : {
                    "dynamic" : true,
                    "type" : "object",
                    "properties" : {
                      "search" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "request" : {
                            "dynamic" : true,
                            "type" : "object",
                            "properties" : {
                              "indices" : {
                                "type" : "keyword"
                              },
                              "types" : {
                                "type" : "keyword"
                              },
                              "search_type" : {
                                "type" : "keyword"
                              }
                            }
                          }
                        }
                      },
                      "payload" : {
                        "type" : "object",
                        "enabled" : false
                      },
                      "http" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "request" : {
                            "dynamic" : true,
                            "type" : "object",
                            "properties" : {
                              "path" : {
                                "type" : "keyword"
                              },
                              "host" : {
                                "type" : "keyword"
                              }
                            }
                          }
                        }
                      },
                      "type" : {
                        "type" : "keyword"
                      },
                      "status" : {
                        "type" : "keyword"
                      }
                    }
                  },
                  "condition" : {
                    "dynamic" : true,
                    "type" : "object",
                    "properties" : {
                      "compare" : {
                        "type" : "object",
                        "enabled" : false
                      },
                      "array_compare" : {
                        "type" : "object",
                        "enabled" : false
                      },
                      "type" : {
                        "type" : "keyword"
                      },
                      "met" : {
                        "type" : "boolean"
                      },
                      "script" : {
                        "type" : "object",
                        "enabled" : false
                      },
                      "status" : {
                        "type" : "keyword"
                      }
                    }
                  },
                  "transform" : {
                    "dynamic" : true,
                    "type" : "object",
                    "properties" : {
                      "search" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "request" : {
                            "dynamic" : true,
                            "type" : "object",
                            "properties" : {
                              "indices" : {
                                "type" : "keyword"
                              },
                              "types" : {
                                "type" : "keyword"
                              }
                            }
                          }
                        }
                      },
                      "type" : {
                        "type" : "keyword"
                      }
                    }
                  },
                  "execution_duration" : {
                    "type" : "long"
                  },
                  "actions" : {
                    "include_in_parent" : true,
                    "dynamic" : true,
                    "type" : "nested",
                    "properties" : {
                      "reason" : {
                        "type" : "keyword"
                      },
                      "foreach" : {
                        "type" : "object",
                        "enabled" : false
                      },
                      "webhook" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "request" : {
                            "dynamic" : true,
                            "type" : "object",
                            "properties" : {
                              "path" : {
                                "type" : "keyword"
                              },
                              "host" : {
                                "type" : "keyword"
                              }
                            }
                          }
                        }
                      },
                      "number_of_actions_executed" : {
                        "type" : "integer"
                      },
                      "slack" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "sent_messages" : {
                            "include_in_parent" : true,
                            "dynamic" : true,
                            "type" : "nested",
                            "properties" : {
                              "reason" : {
                                "type" : "text"
                              },
                              "request" : {
                                "type" : "object",
                                "enabled" : false
                              },
                              "response" : {
                                "type" : "object",
                                "enabled" : false
                              },
                              "to" : {
                                "type" : "keyword"
                              },
                              "message" : {
                                "dynamic" : true,
                                "type" : "object",
                                "properties" : {
                                  "attachments" : {
                                    "include_in_parent" : true,
                                    "dynamic" : true,
                                    "type" : "nested",
                                    "properties" : {
                                      "color" : {
                                        "type" : "keyword"
                                      },
                                      "fields" : {
                                        "properties" : {
                                          "value" : {
                                            "type" : "text"
                                          }
                                        }
                                      }
                                    }
                                  },
                                  "icon" : {
                                    "type" : "keyword"
                                  },
                                  "from" : {
                                    "type" : "text"
                                  },
                                  "text" : {
                                    "type" : "text"
                                  }
                                }
                              },
                              "status" : {
                                "type" : "keyword"
                              }
                            }
                          },
                          "account" : {
                            "type" : "keyword"
                          }
                        }
                      },
                      "index" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "response" : {
                            "dynamic" : true,
                            "type" : "object",
                            "properties" : {
                              "index" : {
                                "type" : "keyword"
                              },
                              "id" : {
                                "type" : "keyword"
                              },
                              "type" : {
                                "type" : "keyword"
                              }
                            }
                          }
                        }
                      },
                      "pagerduty" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "sent_event" : {
                            "include_in_parent" : true,
                            "dynamic" : true,
                            "type" : "nested",
                            "properties" : {
                              "reason" : {
                                "type" : "text"
                              },
                              "request" : {
                                "type" : "object",
                                "enabled" : false
                              },
                              "response" : {
                                "type" : "object",
                                "enabled" : false
                              },
                              "event" : {
                                "dynamic" : true,
                                "type" : "object",
                                "properties" : {
                                  "client_url" : {
                                    "type" : "keyword"
                                  },
                                  "context" : {
                                    "include_in_parent" : true,
                                    "dynamic" : true,
                                    "type" : "nested",
                                    "properties" : {
                                      "src" : {
                                        "type" : "keyword"
                                      },
                                      "alt" : {
                                        "type" : "text"
                                      },
                                      "href" : {
                                        "type" : "keyword"
                                      },
                                      "type" : {
                                        "type" : "keyword"
                                      }
                                    }
                                  },
                                  "client" : {
                                    "type" : "text"
                                  },
                                  "description" : {
                                    "type" : "text"
                                  },
                                  "attach_payload" : {
                                    "type" : "boolean"
                                  },
                                  "incident_key" : {
                                    "type" : "keyword"
                                  },
                                  "type" : {
                                    "type" : "keyword"
                                  },
                                  "account" : {
                                    "type" : "keyword"
                                  }
                                }
                              }
                            }
                          },
                          "account" : {
                            "type" : "keyword"
                          }
                        }
                      },
                      "id" : {
                        "type" : "keyword"
                      },
                      "type" : {
                        "type" : "keyword"
                      },
                      "email" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "message" : {
                            "dynamic" : true,
                            "type" : "object",
                            "properties" : {
                              "cc" : {
                                "type" : "keyword"
                              },
                              "bcc" : {
                                "type" : "keyword"
                              },
                              "reply_to" : {
                                "type" : "keyword"
                              },
                              "from" : {
                                "type" : "keyword"
                              },
                              "id" : {
                                "type" : "keyword"
                              },
                              "to" : {
                                "type" : "keyword"
                              }
                            }
                          }
                        }
                      },
                      "status" : {
                        "type" : "keyword"
                      },
                      "jira" : {
                        "dynamic" : true,
                        "type" : "object",
                        "properties" : {
                          "result" : {
                            "dynamic" : true,
                            "type" : "object",
                            "properties" : {
                              "self" : {
                                "type" : "keyword"
                              },
                              "id" : {
                                "type" : "keyword"
                              },
                              "key" : {
                                "type" : "keyword"
                              }
                            }
                          },
                          "reason" : {
                            "type" : "text"
                          },
                          "request" : {
                            "type" : "object",
                            "enabled" : false
                          },
                          "response" : {
                            "type" : "object",
                            "enabled" : false
                          },
                          "fields" : {
                            "dynamic" : true,
                            "type" : "object",
                            "properties" : {
                              "summary" : {
                                "type" : "text"
                              },
                              "issuetype" : {
                                "dynamic" : true,
                                "type" : "object",
                                "properties" : {
                                  "name" : {
                                    "type" : "keyword"
                                  },
                                  "id" : {
                                    "type" : "keyword"
                                  }
                                }
                              },
                              "description" : {
                                "type" : "text"
                              },
                              "project" : {
                                "dynamic" : true,
                                "type" : "object",
                                "properties" : {
                                  "id" : {
                                    "type" : "keyword"
                                  },
                                  "key" : {
                                    "type" : "keyword"
                                  }
                                }
                              },
                              "labels" : {
                                "type" : "text"
                              }
                            }
                          },
                          "account" : {
                            "type" : "keyword"
                          }
                        }
                      }
                    }
                  },
                  "execution_time" : {
                    "type" : "date"
                  }
                }
              },
              "node" : {
                "type" : "keyword"
              },
              "input" : {
                "type" : "object",
                "enabled" : false
              },
              "condition" : {
                "type" : "object",
                "enabled" : false
              },
              "watch_id" : {
                "type" : "keyword"
              },
              "messages" : {
                "type" : "text"
              },
              "vars" : {
                "type" : "object",
                "enabled" : false
              },
              "state" : {
                "type" : "keyword"
              },
              "user" : {
                "type" : "text"
              },
              "status" : {
                "dynamic" : true,
                "type" : "object",
                "enabled" : false
              }
            }
          }
        },
        "composed_of" : [ ],
        "priority" : 2147483647,
        "version" : 13,
        "_meta" : {
          "managed" : true,
          "description" : "index template for watcher history indices"
        },
        "allow_auto_create" : true
      }
    },
    {
      "name" : ".ml-state",
      "index_template" : {
        "index_patterns" : [
          ".ml-state*"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "hidden" : "true",
              "lifecycle" : {
                "name" : "ml-size-based-ilm-policy",
                "rollover_alias" : ".ml-state-write"
              },
              "auto_expand_replicas" : "0-1"
            }
          },
          "mappings" : {
            "_meta" : {
              "version" : "7172999"
            },
            "enabled" : false
          },
          "aliases" : { }
        },
        "composed_of" : [ ],
        "priority" : 2147483647,
        "version" : 7172999,
        "_meta" : {
          "managed" : true,
          "description" : "index template for ML state indices"
        }
      }
    },
    {
      "name" : "ilm-history",
      "index_template" : {
        "index_patterns" : [
          "ilm-history-5*"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "lifecycle" : {
                "name" : "ilm-history-ilm-policy"
              },
              "number_of_shards" : "1",
              "auto_expand_replicas" : "0-1",
              "number_of_replicas" : "0"
            }
          },
          "mappings" : {
            "dynamic" : false,
            "properties" : {
              "index_age" : {
                "type" : "long"
              },
              "@timestamp" : {
                "format" : "epoch_millis",
                "type" : "date"
              },
              "error_details" : {
                "type" : "text"
              },
              "success" : {
                "type" : "boolean"
              },
              "index" : {
                "type" : "keyword"
              },
              "state" : {
                "dynamic" : true,
                "type" : "object",
                "properties" : {
                  "phase" : {
                    "type" : "keyword"
                  },
                  "failed_step" : {
                    "type" : "keyword"
                  },
                  "phase_definition" : {
                    "type" : "text"
                  },
                  "action_time" : {
                    "format" : "epoch_millis",
                    "type" : "date"
                  },
                  "phase_time" : {
                    "format" : "epoch_millis",
                    "type" : "date"
                  },
                  "step_info" : {
                    "type" : "text"
                  },
                  "action" : {
                    "type" : "keyword"
                  },
                  "step" : {
                    "type" : "keyword"
                  },
                  "is_auto-retryable_error" : {
                    "type" : "keyword"
                  },
                  "creation_date" : {
                    "format" : "epoch_millis",
                    "type" : "date"
                  },
                  "step_time" : {
                    "format" : "epoch_millis",
                    "type" : "date"
                  }
                }
              },
              "policy" : {
                "type" : "keyword"
              }
            }
          }
        },
        "composed_of" : [ ],
        "priority" : 2147483647,
        "version" : 5,
        "_meta" : {
          "managed" : true,
          "description" : "index template for ILM history indices"
        },
        "data_stream" : {
          "hidden" : true
        },
        "allow_auto_create" : true
      }
    },
    {
      "name" : ".slm-history",
      "index_template" : {
        "index_patterns" : [
          ".slm-history-5*"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "lifecycle" : {
                "name" : "slm-history-ilm-policy"
              },
              "number_of_shards" : "1",
              "auto_expand_replicas" : "0-1",
              "number_of_replicas" : "0"
            }
          },
          "mappings" : {
            "dynamic" : false,
            "properties" : {
              "snapshot_name" : {
                "type" : "keyword"
              },
              "@timestamp" : {
                "format" : "epoch_millis",
                "type" : "date"
              },
              "configuration" : {
                "dynamic" : false,
                "type" : "object",
                "properties" : {
                  "indices" : {
                    "type" : "keyword"
                  },
                  "include_global_state" : {
                    "type" : "boolean"
                  },
                  "partial" : {
                    "type" : "boolean"
                  }
                }
              },
              "error_details" : {
                "index" : false,
                "type" : "text"
              },
              "success" : {
                "type" : "boolean"
              },
              "repository" : {
                "type" : "keyword"
              },
              "operation" : {
                "type" : "keyword"
              },
              "policy" : {
                "type" : "keyword"
              }
            }
          }
        },
        "composed_of" : [ ],
        "priority" : 2147483647,
        "version" : 5,
        "_meta" : {
          "managed" : true,
          "description" : "index template for SLM history indices"
        },
        "data_stream" : {
          "hidden" : true
        },
        "allow_auto_create" : true
      }
    },
    {
      "name" : ".kibana-event-log-7.17.29-template",
      "index_template" : {
        "index_patterns" : [
          ".kibana-event-log-7.17.29-*"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "lifecycle" : {
                "name" : "kibana-event-log-policy",
                "rollover_alias" : ".kibana-event-log-7.17.29"
              },
              "hidden" : "true",
              "number_of_shards" : "1",
              "auto_expand_replicas" : "0-1"
            }
          },
          "mappings" : {
            "dynamic" : "false",
            "properties" : {
              "@timestamp" : {
                "type" : "date"
              },
              "ecs" : {
                "properties" : {
                  "version" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  }
                }
              },
              "log" : {
                "properties" : {
                  "level" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "logger" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  }
                }
              },
              "rule" : {
                "properties" : {
                  "reference" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "license" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "author" : {
                    "meta" : {
                      "isArray" : "true"
                    },
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "name" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "ruleset" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "description" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "id" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "category" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "uuid" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "version" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  }
                }
              },
              "message" : {
                "norms" : false,
                "type" : "text"
              },
              "error" : {
                "properties" : {
                  "code" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "id" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "stack_trace" : {
                    "ignore_above" : 1024,
                    "index" : false,
                    "fields" : {
                      "text" : {
                        "norms" : false,
                        "type" : "text"
                      }
                    },
                    "type" : "keyword",
                    "doc_values" : false
                  },
                  "message" : {
                    "norms" : false,
                    "type" : "text"
                  },
                  "type" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  }
                }
              },
              "event" : {
                "properties" : {
                  "reason" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "code" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "timezone" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "type" : {
                    "meta" : {
                      "isArray" : "true"
                    },
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "duration" : {
                    "type" : "long"
                  },
                  "reference" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "ingested" : {
                    "type" : "date"
                  },
                  "provider" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "action" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "end" : {
                    "type" : "date"
                  },
                  "id" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "outcome" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "severity" : {
                    "type" : "long"
                  },
                  "original" : {
                    "ignore_above" : 1024,
                    "index" : false,
                    "type" : "keyword",
                    "doc_values" : false
                  },
                  "risk_score" : {
                    "type" : "float"
                  },
                  "created" : {
                    "type" : "date"
                  },
                  "kind" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "module" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "start" : {
                    "type" : "date"
                  },
                  "url" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "sequence" : {
                    "type" : "long"
                  },
                  "risk_score_norm" : {
                    "type" : "float"
                  },
                  "category" : {
                    "meta" : {
                      "isArray" : "true"
                    },
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "dataset" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "hash" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  }
                }
              },
              "kibana" : {
                "properties" : {
                  "saved_objects" : {
                    "type" : "nested",
                    "properties" : {
                      "type_id" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      },
                      "rel" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      },
                      "namespace" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      },
                      "id" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      },
                      "type" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      }
                    }
                  },
                  "task" : {
                    "properties" : {
                      "schedule_delay" : {
                        "type" : "long"
                      },
                      "scheduled" : {
                        "type" : "date"
                      }
                    }
                  },
                  "alert" : {
                    "properties" : {
                      "rule" : {
                        "properties" : {
                          "execution" : {
                            "properties" : {
                              "status_order" : {
                                "type" : "long"
                              },
                              "metrics" : {
                                "properties" : {
                                  "execution_gap_duration_s" : {
                                    "type" : "long"
                                  },
                                  "total_indexing_duration_ms" : {
                                    "type" : "long"
                                  },
                                  "total_search_duration_ms" : {
                                    "type" : "long"
                                  }
                                }
                              },
                              "uuid" : {
                                "ignore_above" : 1024,
                                "type" : "keyword"
                              },
                              "status" : {
                                "ignore_above" : 1024,
                                "type" : "keyword"
                              }
                            }
                          }
                        }
                      }
                    }
                  },
                  "space_ids" : {
                    "meta" : {
                      "isArray" : "true"
                    },
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "version" : {
                    "type" : "version"
                  },
                  "server_uuid" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  },
                  "alerting" : {
                    "properties" : {
                      "instance_id" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      },
                      "action_subgroup" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      },
                      "action_group_id" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      },
                      "status" : {
                        "ignore_above" : 1024,
                        "type" : "keyword"
                      }
                    }
                  }
                }
              },
              "user" : {
                "properties" : {
                  "name" : {
                    "ignore_above" : 1024,
                    "fields" : {
                      "text" : {
                        "norms" : false,
                        "type" : "text"
                      }
                    },
                    "type" : "keyword"
                  }
                }
              },
              "tags" : {
                "meta" : {
                  "isArray" : "true"
                },
                "ignore_above" : 1024,
                "type" : "keyword"
              }
            }
          }
        },
        "composed_of" : [ ]
      }
    },
    {
      "name" : ".ml-anomalies-",
      "index_template" : {
        "index_patterns" : [
          ".ml-anomalies-*"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "hidden" : "true",
              "translog" : {
                "durability" : "async"
              },
              "auto_expand_replicas" : "0-1",
              "query" : {
                "default_field" : "all_field_values"
              }
            }
          },
          "mappings" : {
            "_meta" : {
              "version" : "7.17.29"
            },
            "dynamic_templates" : [
              {
                "strings_as_keywords" : {
                  "mapping" : {
                    "type" : "keyword"
                  },
                  "match" : "*"
                }
              }
            ],
            "properties" : {
              "search_count" : {
                "type" : "long"
              },
              "bucket_count" : {
                "type" : "long"
              },
              "terms" : {
                "type" : "text"
              },
              "record_score" : {
                "type" : "double"
              },
              "forecast_expiry_timestamp" : {
                "type" : "date"
              },
              "over_field_value" : {
                "copy_to" : [
                  "all_field_values"
                ],
                "type" : "keyword"
              },
              "preferred_to_categories" : {
                "type" : "long"
              },
              "empty_bucket_count" : {
                "type" : "long"
              },
              "forecast_create_timestamp" : {
                "type" : "date"
              },
              "probability" : {
                "type" : "double"
              },
              "missing_field_count" : {
                "type" : "long"
              },
              "forecast_lower" : {
                "type" : "double"
              },
              "influencer_field_value" : {
                "copy_to" : [
                  "all_field_values"
                ],
                "type" : "keyword"
              },
              "multi_bucket_impact" : {
                "type" : "double"
              },
              "forecast_progress" : {
                "type" : "double"
              },
              "max_matching_length" : {
                "type" : "long"
              },
              "latest_record_time_stamp" : {
                "type" : "date"
              },
              "regex" : {
                "type" : "keyword"
              },
              "examples" : {
                "type" : "text"
              },
              "snapshot_doc_count" : {
                "type" : "integer"
              },
              "average_bucket_processing_time_ms" : {
                "type" : "double"
              },
              "initial_anomaly_score" : {
                "type" : "double"
              },
              "initial_influencer_score" : {
                "type" : "double"
              },
              "partition_field_value" : {
                "copy_to" : [
                  "all_field_values"
                ],
                "type" : "keyword"
              },
              "model_lower" : {
                "type" : "double"
              },
              "total_over_field_count" : {
                "type" : "long"
              },
              "retain" : {
                "type" : "boolean"
              },
              "forecast_upper" : {
                "type" : "double"
              },
              "latest_sparse_bucket_timestamp" : {
                "type" : "date"
              },
              "model_median" : {
                "type" : "double"
              },
              "category_id" : {
                "type" : "long"
              },
              "causes" : {
                "type" : "nested",
                "properties" : {
                  "actual" : {
                    "type" : "double"
                  },
                  "partition_field_name" : {
                    "type" : "keyword"
                  },
                  "partition_field_value" : {
                    "copy_to" : [
                      "all_field_values"
                    ],
                    "type" : "keyword"
                  },
                  "by_field_name" : {
                    "type" : "keyword"
                  },
                  "probability" : {
                    "type" : "double"
                  },
                  "geo_results" : {
                    "properties" : {
                      "actual_point" : {
                        "type" : "geo_point"
                      },
                      "typical_point" : {
                        "type" : "geo_point"
                      }
                    }
                  },
                  "field_name" : {
                    "type" : "keyword"
                  },
                  "by_field_value" : {
                    "copy_to" : [
                      "all_field_values"
                    ],
                    "type" : "keyword"
                  },
                  "over_field_name" : {
                    "type" : "keyword"
                  },
                  "correlated_by_field_value" : {
                    "copy_to" : [
                      "all_field_values"
                    ],
                    "type" : "keyword"
                  },
                  "function" : {
                    "type" : "keyword"
                  },
                  "typical" : {
                    "type" : "double"
                  },
                  "over_field_value" : {
                    "copy_to" : [
                      "all_field_values"
                    ],
                    "type" : "keyword"
                  },
                  "function_description" : {
                    "type" : "keyword"
                  }
                }
              },
              "all_field_values" : {
                "analyzer" : "whitespace",
                "type" : "text"
              },
              "timestamp" : {
                "type" : "date"
              },
              "input_field_count" : {
                "type" : "long"
              },
              "model_bytes" : {
                "type" : "long"
              },
              "quantiles" : {
                "type" : "object",
                "enabled" : false
              },
              "function_description" : {
                "type" : "keyword"
              },
              "min_version" : {
                "type" : "keyword"
              },
              "raw_anomaly_score" : {
                "type" : "double"
              },
              "exponential_average_bucket_processing_time_ms" : {
                "type" : "double"
              },
              "invalid_date_count" : {
                "type" : "long"
              },
              "snapshot_id" : {
                "type" : "keyword"
              },
              "model_size_stats" : {
                "properties" : {
                  "model_bytes" : {
                    "type" : "long"
                  },
                  "result_type" : {
                    "type" : "keyword"
                  },
                  "peak_model_bytes" : {
                    "type" : "long"
                  },
                  "job_id" : {
                    "type" : "keyword"
                  },
                  "total_over_field_count" : {
                    "type" : "long"
                  },
                  "total_partition_field_count" : {
                    "type" : "long"
                  },
                  "total_by_field_count" : {
                    "type" : "long"
                  },
                  "assignment_memory_basis" : {
                    "type" : "keyword"
                  },
                  "bucket_allocation_failures_count" : {
                    "type" : "long"
                  },
                  "memory_status" : {
                    "type" : "keyword"
                  },
                  "log_time" : {
                    "type" : "date"
                  },
                  "timestamp" : {
                    "type" : "date"
                  }
                }
              },
              "total_search_time_ms" : {
                "type" : "double"
              },
              "latest_empty_bucket_timestamp" : {
                "type" : "date"
              },
              "anomaly_score" : {
                "type" : "double"
              },
              "over_field_name" : {
                "type" : "keyword"
              },
              "earliest_record_timestamp" : {
                "type" : "date"
              },
              "scheduled_events" : {
                "type" : "keyword"
              },
              "bucket_span" : {
                "type" : "long"
              },
              "maximum_bucket_processing_time_ms" : {
                "type" : "double"
              },
              "exponential_average_calculation_context" : {
                "properties" : {
                  "incremental_metric_value_ms" : {
                    "type" : "double"
                  },
                  "previous_exponential_average_ms" : {
                    "type" : "double"
                  },
                  "latest_timestamp" : {
                    "type" : "date"
                  }
                }
              },
              "function" : {
                "type" : "keyword"
              },
              "influencers" : {
                "type" : "nested",
                "properties" : {
                  "influencer_field_name" : {
                    "type" : "keyword"
                  },
                  "influencer_field_values" : {
                    "copy_to" : [
                      "all_field_values"
                    ],
                    "type" : "keyword"
                  }
                }
              },
              "input_record_count" : {
                "type" : "long"
              },
              "latest_result_time_stamp" : {
                "type" : "date"
              },
              "model_upper" : {
                "type" : "double"
              },
              "actual" : {
                "type" : "double"
              },
              "forecast_memory_bytes" : {
                "type" : "long"
              },
              "total_by_field_count" : {
                "type" : "long"
              },
              "geo_results" : {
                "properties" : {
                  "actual_point" : {
                    "type" : "geo_point"
                  },
                  "typical_point" : {
                    "type" : "geo_point"
                  }
                }
              },
              "detector_index" : {
                "type" : "integer"
              },
              "by_field_value" : {
                "copy_to" : [
                  "all_field_values"
                ],
                "type" : "keyword"
              },
              "processed_record_count" : {
                "type" : "long"
              },
              "total_partition_field_count" : {
                "type" : "long"
              },
              "mlcategory" : {
                "type" : "keyword"
              },
              "assignment_memory_basis" : {
                "type" : "keyword"
              },
              "forecast_end_timestamp" : {
                "type" : "date"
              },
              "forecast_id" : {
                "type" : "keyword"
              },
              "partition_field_name" : {
                "type" : "keyword"
              },
              "by_field_name" : {
                "type" : "keyword"
              },
              "event_count" : {
                "type" : "long"
              },
              "description" : {
                "type" : "text"
              },
              "is_interim" : {
                "type" : "boolean"
              },
              "bucket_allocation_failures_count" : {
                "type" : "long"
              },
              "memory_status" : {
                "type" : "keyword"
              },
              "model_feature" : {
                "type" : "keyword"
              },
              "num_matches" : {
                "type" : "long"
              },
              "influencer_score" : {
                "type" : "double"
              },
              "out_of_order_timestamp_count" : {
                "type" : "long"
              },
              "result_type" : {
                "type" : "keyword"
              },
              "last_data_time" : {
                "type" : "date"
              },
              "latest_record_timestamp" : {
                "type" : "date"
              },
              "influencer_field_name" : {
                "type" : "keyword"
              },
              "forecast_messages" : {
                "type" : "keyword"
              },
              "sparse_bucket_count" : {
                "type" : "long"
              },
              "log_time" : {
                "type" : "date"
              },
              "field_name" : {
                "type" : "keyword"
              },
              "minimum_bucket_processing_time_ms" : {
                "type" : "double"
              },
              "bucket_influencers" : {
                "type" : "nested",
                "properties" : {
                  "anomaly_score" : {
                    "type" : "double"
                  },
                  "initial_anomaly_score" : {
                    "type" : "double"
                  },
                  "result_type" : {
                    "type" : "keyword"
                  },
                  "raw_anomaly_score" : {
                    "type" : "double"
                  },
                  "bucket_span" : {
                    "type" : "long"
                  },
                  "job_id" : {
                    "type" : "keyword"
                  },
                  "probability" : {
                    "type" : "double"
                  },
                  "influencer_field_name" : {
                    "type" : "keyword"
                  },
                  "is_interim" : {
                    "type" : "boolean"
                  },
                  "timestamp" : {
                    "type" : "date"
                  }
                }
              },
              "processing_time_ms" : {
                "type" : "long"
              },
              "input_bytes" : {
                "type" : "long"
              },
              "initial_record_score" : {
                "type" : "double"
              },
              "job_id" : {
                "copy_to" : [
                  "all_field_values"
                ],
                "type" : "keyword"
              },
              "processed_field_count" : {
                "type" : "long"
              },
              "forecast_status" : {
                "type" : "keyword"
              },
              "typical" : {
                "type" : "double"
              },
              "forecast_prediction" : {
                "type" : "double"
              },
              "forecast_start_timestamp" : {
                "type" : "date"
              }
            }
          }
        },
        "composed_of" : [ ],
        "priority" : 2147483647,
        "version" : 7172999,
        "_meta" : {
          "managed" : true,
          "description" : "index template for ML anomaly detection results indices"
        }
      }
    },
    {
      "name" : "metrics",
      "index_template" : {
        "index_patterns" : [
          "metrics-*-*"
        ],
        "composed_of" : [
          "metrics-mappings",
          "data-streams-mappings",
          "metrics-settings"
        ],
        "priority" : 100,
        "version" : 1,
        "_meta" : {
          "managed" : true,
          "description" : "default metrics template installed by x-pack"
        },
        "data_stream" : {
          "hidden" : false
        },
        "allow_auto_create" : true
      }
    },
    {
      "name" : ".ml-notifications-000002",
      "index_template" : {
        "index_patterns" : [
          ".ml-notifications-000002"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "hidden" : "true",
              "number_of_shards" : "1",
              "auto_expand_replicas" : "0-1"
            }
          },
          "mappings" : {
            "_meta" : {
              "version" : "7.17.29"
            },
            "dynamic" : "false",
            "properties" : {
              "job_type" : {
                "type" : "keyword"
              },
              "level" : {
                "type" : "keyword"
              },
              "job_id" : {
                "type" : "keyword"
              },
              "node_name" : {
                "type" : "keyword"
              },
              "message" : {
                "type" : "text",
                "fields" : {
                  "raw" : {
                    "ignore_above" : 1024,
                    "type" : "keyword"
                  }
                }
              },
              "cleared" : {
                "type" : "boolean"
              },
              "timestamp" : {
                "type" : "date"
              }
            }
          }
        },
        "composed_of" : [ ],
        "priority" : 2147483647,
        "version" : 7172999,
        "_meta" : {
          "managed" : true,
          "description" : "index template for ML notifications indices"
        }
      }
    },
    {
      "name" : ".deprecation-indexing-template",
      "index_template" : {
        "index_patterns" : [
          ".logs-deprecation.*"
        ],
        "composed_of" : [
          ".deprecation-indexing-mappings",
          ".deprecation-indexing-settings"
        ],
        "priority" : 1000,
        "version" : 1,
        "_meta" : {
          "managed" : true,
          "description" : "default template for Stack deprecation logs index template installed by x-pack"
        },
        "data_stream" : {
          "hidden" : true
        },
        "allow_auto_create" : true
      }
    },
    {
      "name" : "logs",
      "index_template" : {
        "index_patterns" : [
          "logs-*-*"
        ],
        "composed_of" : [
          "logs-mappings",
          "data-streams-mappings",
          "logs-settings"
        ],
        "priority" : 100,
        "version" : 1,
        "_meta" : {
          "managed" : true,
          "description" : "default logs template installed by x-pack"
        },
        "data_stream" : {
          "hidden" : false
        },
        "allow_auto_create" : true
      }
    },
    {
      "name" : ".ml-stats",
      "index_template" : {
        "index_patterns" : [
          ".ml-stats-*"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "lifecycle" : {
                "name" : "ml-size-based-ilm-policy",
                "rollover_alias" : ".ml-stats-write"
              },
              "hidden" : "true",
              "number_of_shards" : "1",
              "auto_expand_replicas" : "0-1"
            }
          },
          "mappings" : {
            "_meta" : {
              "version" : "7.17.29"
            },
            "dynamic" : false,
            "properties" : {
              "skipped_docs_count" : {
                "type" : "long"
              },
              "validation_loss" : {
                "properties" : {
                  "fold_values" : {
                    "properties" : {
                      "fold" : {
                        "type" : "integer"
                      },
                      "values" : {
                        "type" : "double"
                      }
                    }
                  },
                  "loss_type" : {
                    "type" : "keyword"
                  }
                }
              },
              "cache_miss_count" : {
                "type" : "long"
              },
              "timing_stats" : {
                "properties" : {
                  "iteration_time" : {
                    "type" : "long"
                  },
                  "elapsed_time" : {
                    "type" : "long"
                  }
                }
              },
              "failure_count" : {
                "type" : "long"
              },
              "model_id" : {
                "type" : "keyword"
              },
              "type" : {
                "type" : "keyword"
              },
              "training_docs_count" : {
                "type" : "long"
              },
              "inference_count" : {
                "type" : "long"
              },
              "job_id" : {
                "type" : "keyword"
              },
              "missing_all_fields_count" : {
                "type" : "long"
              },
              "peak_usage_bytes" : {
                "type" : "long"
              },
              "iteration" : {
                "type" : "integer"
              },
              "hyperparameters" : {
                "properties" : {
                  "max_attempts_to_add_tree" : {
                    "type" : "integer"
                  },
                  "downsample_factor" : {
                    "type" : "double"
                  },
                  "eta_growth_rate_per_tree" : {
                    "type" : "double"
                  },
                  "soft_tree_depth_tolerance" : {
                    "type" : "double"
                  },
                  "max_trees" : {
                    "type" : "integer"
                  },
                  "lambda" : {
                    "type" : "double"
                  },
                  "max_optimization_rounds_per_hyperparameter" : {
                    "type" : "integer"
                  },
                  "eta" : {
                    "type" : "double"
                  },
                  "soft_tree_depth_limit" : {
                    "type" : "double"
                  },
                  "alpha" : {
                    "type" : "double"
                  },
                  "class_assignment_objective" : {
                    "type" : "keyword"
                  },
                  "feature_bag_fraction" : {
                    "type" : "double"
                  },
                  "num_splits_per_feature" : {
                    "type" : "integer"
                  },
                  "gamma" : {
                    "type" : "double"
                  },
                  "num_folds" : {
                    "type" : "integer"
                  }
                }
              },
              "parameters" : {
                "properties" : {
                  "compute_feature_influence" : {
                    "type" : "boolean"
                  },
                  "feature_influence_threshold" : {
                    "type" : "double"
                  },
                  "outlier_fraction" : {
                    "type" : "double"
                  },
                  "method" : {
                    "type" : "keyword"
                  },
                  "standardization_enabled" : {
                    "type" : "boolean"
                  },
                  "n_neighbors" : {
                    "type" : "integer"
                  }
                }
              },
              "test_docs_count" : {
                "type" : "long"
              },
              "node_id" : {
                "type" : "keyword"
              },
              "timestamp" : {
                "type" : "date"
              }
            }
          },
          "aliases" : { }
        },
        "composed_of" : [ ],
        "priority" : 2147483647,
        "version" : 7172999,
        "_meta" : {
          "managed" : true,
          "description" : "index template for ML stats indices"
        }
      }
    },
    {
      "name" : "sample_syslog_fwlogs_template",
      "index_template" : {
        "index_patterns" : [
          "sample_syslog_fwlogs-*"
        ],
        "template" : {
          "settings" : {
            "index" : {
              "lifecycle" : {
                "name" : "sample_syslog_fwlogs_ilm_policy",
                "rollover_alias" : "sample_syslog_fwlogs"
              }
            }
          },
          "mappings" : {
            "properties" : {
              "app" : {
                "type" : "keyword"
              },
              "threat_level" : {
                "type" : "keyword"
              },
              "bytes_received" : {
                "type" : "long"
              },
              "interface" : {
                "type" : "keyword"
              },
              "bytes_sent" : {
                "type" : "long"
              },
              "dst_ip" : {
                "type" : "ip"
              },
              "src_ip" : {
                "type" : "ip"
              },
              "src_port" : {
                "type" : "integer"
              },
              "rule_id" : {
                "type" : "integer"
              },
              "duration" : {
                "type" : "float"
              },
              "duration_ms" : {
                "type" : "keyword"
              },
              "protocol" : {
                "type" : "keyword"
              },
              "hostname" : {
                "type" : "keyword"
              },
              "@timestamp" : {
                "type" : "date"
              },
              "event_type" : {
                "type" : "keyword"
              },
              "dst_port" : {
                "type" : "integer"
              },
              "user" : {
                "type" : "keyword"
              }
            }
          }
        },
        "composed_of" : [ ]
      }
    }
  ]
}
