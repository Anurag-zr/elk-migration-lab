```bash
GET _ilm/policy
```

{
  ".alerts-ilm-policy" : {
    "version" : 7,
    "modified_date" : "2025-08-11T04:08:17.821Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        }
      },
      "_meta" : {
        "managed" : true
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [ ]
    }
  },
  ".deprecation-indexing-ilm-policy" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.463Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "10gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "30d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "ILM policy used for deprecation logs"
      }
    },
    "in_use_by" : {
      "indices" : [
        ".ds-.logs-deprecation.elasticsearch-default-2025.08.01-000001"
      ],
      "data_streams" : [
        ".logs-deprecation.elasticsearch-default"
      ],
      "composable_templates" : [
        ".deprecation-indexing-template"
      ]
    }
  },
  ".fleet-actions-results-ilm-policy" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.475Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_size" : "300gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "90d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "default policy for fleet action results indices"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [ ]
    }
  },
  "180-days-default" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.331Z",
    "policy" : {
      "phases" : {
        "warm" : {
          "min_age" : "2d",
          "actions" : {
            "shrink" : {
              "number_of_shards" : 1
            },
            "forcemerge" : {
              "max_num_segments" : 1
            }
          }
        },
        "cold" : {
          "min_age" : "30d",
          "actions" : { }
        },
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "180d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "built-in ILM policy using the hot, warm, and cold phases with a retention of 180 days"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [ ]
    }
  },
  "30-days-default" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.396Z",
    "policy" : {
      "phases" : {
        "warm" : {
          "min_age" : "2d",
          "actions" : {
            "shrink" : {
              "number_of_shards" : 1
            },
            "forcemerge" : {
              "max_num_segments" : 1
            }
          }
        },
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "30d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "built-in ILM policy using the hot and warm phases with a retention of 30 days"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [ ]
    }
  },
  "365-days-default" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.383Z",
    "policy" : {
      "phases" : {
        "warm" : {
          "min_age" : "2d",
          "actions" : {
            "shrink" : {
              "number_of_shards" : 1
            },
            "forcemerge" : {
              "max_num_segments" : 1
            }
          }
        },
        "cold" : {
          "min_age" : "30d",
          "actions" : { }
        },
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "365d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "built-in ILM policy using the hot, warm, and cold phases with a retention of 365 days"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [ ]
    }
  },
  "7-days-default" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.352Z",
    "policy" : {
      "phases" : {
        "warm" : {
          "min_age" : "2d",
          "actions" : {
            "shrink" : {
              "number_of_shards" : 1
            },
            "forcemerge" : {
              "max_num_segments" : 1
            }
          }
        },
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "7d"
            }
          }
        },
        "delete" : {
          "min_age" : "7d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "built-in ILM policy using the hot and warm phases with a retention of 7 days"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [ ]
    }
  },
  "90-days-default" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.368Z",
    "policy" : {
      "phases" : {
        "warm" : {
          "min_age" : "2d",
          "actions" : {
            "shrink" : {
              "number_of_shards" : 1
            },
            "forcemerge" : {
              "max_num_segments" : 1
            }
          }
        },
        "cold" : {
          "min_age" : "30d",
          "actions" : { }
        },
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "90d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "built-in ILM policy using the hot, warm, and cold phases with a retention of 90 days"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [ ]
    }
  },
  "ilm-history-ilm-policy" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.438Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "90d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "default policy for the ILM history indices"
      }
    },
    "in_use_by" : {
      "indices" : [
        ".ds-ilm-history-5-2025.08.01-000001"
      ],
      "data_streams" : [
        "ilm-history-5"
      ],
      "composable_templates" : [
        "ilm-history"
      ]
    }
  },
  "kibana-event-log-policy" : {
    "version" : 1,
    "modified_date" : "2025-08-01T04:42:20.445Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_size" : "50gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "90d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      }
    },
    "in_use_by" : {
      "indices" : [
        ".kibana-event-log-7.17.29-000001"
      ],
      "data_streams" : [ ],
      "composable_templates" : [
        ".kibana-event-log-7.17.29-template"
      ]
    }
  },
  "kibana-reporting" : {
    "version" : 1,
    "modified_date" : "2025-08-01T04:42:20.460Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : { }
        }
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [ ]
    }
  },
  "logs" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.411Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "default policy for the logs index template installed by x-pack"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [
        "logs"
      ]
    }
  },
  "metrics" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.299Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "default policy for the metrics index template installed by x-pack"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [
        "metrics"
      ]
    }
  },
  "ml-size-based-ilm-policy" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.276Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb"
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "default policy for machine learning state and stats indices"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [
        ".ml-state",
        ".ml-stats"
      ]
    }
  },
  "sample_syslog_fwlogs_ilm_policy" : {
    "version" : 1,
    "modified_date" : "2025-08-03T07:36:01.499Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_size" : "2gb",
              "max_age" : "7d"
            }
          }
        },
        "delete" : {
          "min_age" : "30d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      }
    },
    "in_use_by" : {
      "indices" : [
        "sample_syslog_fwlogs-2025.08.11-000003",
        "sample_syslog_fwlogs-2025.08.04-000002"
      ],
      "data_streams" : [ ],
      "composable_templates" : [
        "sample_syslog_fwlogs_template"
      ]
    }
  },
  "slm-history-ilm-policy" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.451Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        },
        "delete" : {
          "min_age" : "90d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "default policy for the SLM history indices"
      }
    },
    "in_use_by" : {
      "indices" : [
        ".ds-.slm-history-5-2025.08.06-000001"
      ],
      "data_streams" : [
        ".slm-history-5"
      ],
      "composable_templates" : [
        ".slm-history"
      ]
    }
  },
  "synthetics" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.315Z",
    "policy" : {
      "phases" : {
        "hot" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_primary_shard_size" : "50gb",
              "max_age" : "30d"
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "default policy for the synthetics index template installed by x-pack"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [
        "synthetics"
      ]
    }
  },
  "watch-history-ilm-policy" : {
    "version" : 1,
    "modified_date" : "2025-08-01T03:17:47.425Z",
    "policy" : {
      "phases" : {
        "delete" : {
          "min_age" : "7d",
          "actions" : {
            "delete" : {
              "delete_searchable_snapshot" : true
            }
          }
        }
      },
      "_meta" : {
        "managed" : true,
        "description" : "default policy for the watcher history indices"
      }
    },
    "in_use_by" : {
      "indices" : [ ],
      "data_streams" : [ ],
      "composable_templates" : [
        ".watch-history-13"
      ]
    }
  }
}
