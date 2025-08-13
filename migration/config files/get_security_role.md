```http
GET _secuity/role
```

{
  "kibana_dashboard_only_user" : {
    "cluster" : [ ],
    "indices" : [ ],
    "applications" : [
      {
        "application" : "kibana-.kibana",
        "privileges" : [
          "read"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_deprecated" : true,
      "_reserved" : true,
      "_deprecated_reason" : "Please use Kibana feature privileges instead"
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "apm_system" : {
    "cluster" : [
      "monitor",
      "cluster:admin/xpack/monitoring/bulk"
    ],
    "indices" : [
      {
        "names" : [
          ".monitoring-beats-*"
        ],
        "privileges" : [
          "create_index",
          "create_doc"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "watcher_admin" : {
    "cluster" : [
      "manage_watcher"
    ],
    "indices" : [
      {
        "names" : [
          ".watches",
          ".triggered_watches",
          ".watcher-history-*"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "viewer" : {
    "cluster" : [ ],
    "indices" : [
      {
        "names" : [
          "/~(([.]|ilm-history-).*)/"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".siem-signals-*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [
      {
        "application" : "kibana-.kibana",
        "privileges" : [
          "read"
        ],
        "resources" : [
          "*"
        ]
      },
      {
        "application" : "kibana-*",
        "privileges" : [
          "reserved_ml_user"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "logstash_system" : {
    "cluster" : [
      "monitor",
      "cluster:admin/xpack/monitoring/bulk"
    ],
    "indices" : [ ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "rollup_user" : {
    "cluster" : [
      "monitor_rollup"
    ],
    "indices" : [ ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "kibana_user" : {
    "cluster" : [ ],
    "indices" : [ ],
    "applications" : [
      {
        "application" : "kibana-.kibana",
        "privileges" : [
          "all"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_deprecated" : true,
      "_reserved" : true,
      "_deprecated_reason" : "Please use the [kibana_admin] role instead"
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "beats_admin" : {
    "cluster" : [ ],
    "indices" : [
      {
        "names" : [
          ".management-beats"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "remote_monitoring_agent" : {
    "cluster" : [
      "manage_index_templates",
      "manage_ingest_pipelines",
      "monitor",
      "cluster:admin/ilm/get",
      "cluster:admin/ilm/put",
      "cluster:monitor/xpack/watcher/watch/get",
      "cluster:admin/xpack/watcher/watch/put",
      "cluster:admin/xpack/watcher/watch/delete"
    ],
    "indices" : [
      {
        "names" : [
          ".monitoring-*"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metricbeat-*"
        ],
        "privileges" : [
          "index",
          "create_index",
          "view_index_metadata",
          "indices:admin/aliases"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "rollup_admin" : {
    "cluster" : [
      "manage_rollup"
    ],
    "indices" : [ ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "data_frame_transforms_admin" : {
    "cluster" : [
      "manage_data_frame_transforms"
    ],
    "indices" : [
      {
        "names" : [
          ".transform-notifications-*",
          ".data-frame-notifications-*",
          ".transform-notifications-read"
        ],
        "privileges" : [
          "view_index_metadata",
          "read"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [
      {
        "application" : "kibana-*",
        "privileges" : [
          "reserved_ml_user"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_deprecated" : true,
      "_reserved" : true,
      "_deprecated_reason" : "Please use the [transform_admin] role instead"
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "snapshot_user" : {
    "cluster" : [
      "create_snapshot",
      "cluster:admin/repository/get"
    ],
    "indices" : [
      {
        "names" : [
          "*"
        ],
        "privileges" : [
          "view_index_metadata"
        ],
        "allow_restricted_indices" : true
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "monitoring_user" : {
    "cluster" : [
      "cluster:monitor/main",
      "cluster:monitor/xpack/info",
      "cluster:monitor/remote/info"
    ],
    "indices" : [
      {
        "names" : [
          ".monitoring-*"
        ],
        "privileges" : [
          "read",
          "read_cross_cluster"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metricbeat-*"
        ],
        "privileges" : [
          "read",
          "read_cross_cluster"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [
      {
        "application" : "kibana-*",
        "privileges" : [
          "reserved_monitoring"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "enrich_user" : {
    "cluster" : [
      "manage_enrich",
      "manage_ingest_pipelines",
      "monitor"
    ],
    "indices" : [
      {
        "names" : [
          ".enrich-*"
        ],
        "privileges" : [
          "manage",
          "read",
          "write"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "kibana_admin" : {
    "cluster" : [ ],
    "indices" : [ ],
    "applications" : [
      {
        "application" : "kibana-.kibana",
        "privileges" : [
          "all"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "logstash_admin" : {
    "cluster" : [
      "manage_logstash_pipelines"
    ],
    "indices" : [
      {
        "names" : [
          ".logstash*"
        ],
        "privileges" : [
          "create",
          "delete",
          "index",
          "manage",
          "read"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "editor" : {
    "cluster" : [ ],
    "indices" : [
      {
        "names" : [
          "/~(([.]|ilm-history-).*)/"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "observability-annotations"
        ],
        "privileges" : [
          "read",
          "view_index_metadata",
          "write"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".siem-signals-*",
          ".lists-*",
          ".items-*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata",
          "write",
          "maintenance"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [
      {
        "application" : "kibana-.kibana",
        "privileges" : [
          "all"
        ],
        "resources" : [
          "*"
        ]
      },
      {
        "application" : "kibana-*",
        "privileges" : [
          "reserved_ml_admin"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "machine_learning_user" : {
    "cluster" : [
      "monitor_ml"
    ],
    "indices" : [
      {
        "names" : [
          ".ml-anomalies*",
          ".ml-notifications*"
        ],
        "privileges" : [
          "view_index_metadata",
          "read"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".ml-annotations*"
        ],
        "privileges" : [
          "view_index_metadata",
          "read",
          "write"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [
      {
        "application" : "kibana-*",
        "privileges" : [
          "reserved_ml_user"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "data_frame_transforms_user" : {
    "cluster" : [
      "monitor_data_frame_transforms"
    ],
    "indices" : [
      {
        "names" : [
          ".transform-notifications-*",
          ".data-frame-notifications-*",
          ".transform-notifications-read"
        ],
        "privileges" : [
          "view_index_metadata",
          "read"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [
      {
        "application" : "kibana-*",
        "privileges" : [
          "reserved_ml_user"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_deprecated" : true,
      "_reserved" : true,
      "_deprecated_reason" : "Please use the [transform_user] role instead"
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "machine_learning_admin" : {
    "cluster" : [
      "manage_ml"
    ],
    "indices" : [
      {
        "names" : [
          ".ml-anomalies*",
          ".ml-notifications*",
          ".ml-state*",
          ".ml-meta*",
          ".ml-stats-*"
        ],
        "privileges" : [
          "view_index_metadata",
          "read"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".ml-annotations*"
        ],
        "privileges" : [
          "view_index_metadata",
          "read",
          "write"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [
      {
        "application" : "kibana-*",
        "privileges" : [
          "reserved_ml_admin"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "watcher_user" : {
    "cluster" : [
      "monitor_watcher"
    ],
    "indices" : [
      {
        "names" : [
          ".watches"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".watcher-history-*"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "apm_user" : {
    "cluster" : [ ],
    "indices" : [
      {
        "names" : [
          "apm-*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "logs-apm.*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "logs-apm-*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metrics-apm.*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metrics-apm-*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "traces-apm.*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "traces-apm-*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".ml-anomalies*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "observability-annotations"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [
      {
        "application" : "kibana-*",
        "privileges" : [
          "reserved_ml_apm_user"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [ ],
    "metadata" : {
      "_deprecated" : true,
      "_reserved" : true,
      "_deprecated_reason" : "This role will be removed in 8.0"
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "beats_system" : {
    "cluster" : [
      "monitor",
      "cluster:admin/xpack/monitoring/bulk"
    ],
    "indices" : [
      {
        "names" : [
          ".monitoring-beats-*"
        ],
        "privileges" : [
          "create_index",
          "create"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "reporting_user" : {
    "cluster" : [ ],
    "indices" : [ ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_deprecated" : true,
      "_reserved" : true,
      "_deprecated_reason" : "Please use Kibana feature privileges instead"
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "transform_user" : {
    "cluster" : [
      "monitor_transform"
    ],
    "indices" : [
      {
        "names" : [
          ".transform-notifications-*",
          ".data-frame-notifications-*",
          ".transform-notifications-read"
        ],
        "privileges" : [
          "view_index_metadata",
          "read"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "kibana_system" : {
    "cluster" : [
      "monitor",
      "manage_index_templates",
      "cluster:admin/xpack/monitoring/bulk",
      "manage_saml",
      "manage_token",
      "manage_oidc",
      "manage_pipeline",
      "manage_ilm",
      "manage_transform",
      "cluster:admin/xpack/security/api_key/invalidate",
      "grant_api_key",
      "manage_own_api_key",
      "cluster:admin/xpack/security/privilege/builtin/get",
      "delegate_pki",
      "manage_ml",
      "cluster:admin/analyze",
      "monitor_text_structure",
      "cancel_task"
    ],
    "global" : {
      "application" : {
        "manage" : {
          "applications" : [
            "kibana-*"
          ]
        }
      }
    },
    "indices" : [
      {
        "names" : [
          ".kibana*",
          ".reporting-*"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".monitoring-*"
        ],
        "privileges" : [
          "read",
          "read_cross_cluster"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".management-beats"
        ],
        "privileges" : [
          "create_index",
          "read",
          "write"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".ml-anomalies*",
          ".ml-stats-*"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".ml-annotations*",
          ".ml-notifications*"
        ],
        "privileges" : [
          "read",
          "write"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".apm-agent-configuration"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".apm-custom-link"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "apm-*"
        ],
        "privileges" : [
          "read",
          "read_cross_cluster"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "logs-apm.*"
        ],
        "privileges" : [
          "read",
          "read_cross_cluster"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metrics-apm.*"
        ],
        "privileges" : [
          "read",
          "read_cross_cluster"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "traces-apm.*"
        ],
        "privileges" : [
          "read",
          "read_cross_cluster"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "traces-apm-*"
        ],
        "privileges" : [
          "read",
          "read_cross_cluster"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "*"
        ],
        "privileges" : [
          "view_index_metadata",
          "monitor"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".logs-endpoint.diagnostic.collection-*"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".fleet*"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".siem-signals*"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".internal.alerts*"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".alerts*"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metrics-endpoint.policy-*"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metrics-endpoint.metrics-*"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "logs-*",
          "synthetics-*",
          "traces-*",
          """/metrics-.*&~(metrics-endpoint\.metadata_current_default)/""",
          ".logs-endpoint.action.responses-*",
          ".logs-endpoint.diagnostic.collection-*",
          ".logs-endpoint.actions-*"
        ],
        "privileges" : [
          "indices:admin/settings/update",
          "indices:admin/mapping/put",
          "indices:admin/rollover"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".logs-endpoint.action.responses-*"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".logs-endpoint.actions-*"
        ],
        "privileges" : [
          "auto_configure",
          "read",
          "write"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          ".logs-endpoint.diagnostic.collection-*",
          "traces-apm.sampled-*"
        ],
        "privileges" : [
          "indices:admin/delete"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metrics-endpoint.metadata*"
        ],
        "privileges" : [
          "read",
          "view_index_metadata"
        ],
        "allow_restricted_indices" : false
      },
      {
        "names" : [
          "metrics-endpoint.metadata_current_default",
          ".metrics-endpoint.metadata_current_default",
          ".metrics-endpoint.metadata_united_default"
        ],
        "privileges" : [
          "create_index",
          "delete_index",
          "read",
          "index"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "transform_admin" : {
    "cluster" : [
      "manage_transform"
    ],
    "indices" : [
      {
        "names" : [
          ".transform-notifications-*",
          ".data-frame-notifications-*",
          ".transform-notifications-read"
        ],
        "privileges" : [
          "view_index_metadata",
          "read"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "transport_client" : {
    "cluster" : [
      "transport_client"
    ],
    "indices" : [ ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "remote_monitoring_collector" : {
    "cluster" : [
      "monitor"
    ],
    "indices" : [
      {
        "names" : [
          "*"
        ],
        "privileges" : [
          "monitor"
        ],
        "allow_restricted_indices" : true
      },
      {
        "names" : [
          ".kibana*"
        ],
        "privileges" : [
          "read"
        ],
        "allow_restricted_indices" : false
      }
    ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  },
  "superuser" : {
    "cluster" : [
      "all"
    ],
    "indices" : [
      {
        "names" : [
          "*"
        ],
        "privileges" : [
          "all"
        ],
        "allow_restricted_indices" : true
      }
    ],
    "applications" : [
      {
        "application" : "*",
        "privileges" : [
          "*"
        ],
        "resources" : [
          "*"
        ]
      }
    ],
    "run_as" : [
      "*"
    ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : { }
  },
  "ingest_admin" : {
    "cluster" : [
      "manage_index_templates",
      "manage_pipeline"
    ],
    "indices" : [ ],
    "applications" : [ ],
    "run_as" : [ ],
    "metadata" : {
      "_reserved" : true
    },
    "transient_metadata" : {
      "enabled" : true
    }
  }
}
