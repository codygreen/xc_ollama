# Ollama on F5 Distributed Cloud

## Deploy Ollama

```shell
kubectl apply -f ./ollama
```

## Create an XC Origin Pool

Create an origin pool with the following settings, replace the site name with yours:

```json
"get_spec": {
    "origin_servers": [
      {
        "k8s_service": {
          "service_name": "ollama.ollama",
          "site_locator": {
            "site": {
              "tenant": "your-xc-tenant",
              "namespace": "system",
              "name": "cody-ai-nvidia-t4"
            }
          },
          "inside_network": {}
        },
        "labels": {}
      }
    ],
    "no_tls": {},
    "port": 11434,
    "same_as_endpoint_port": {},
    "healthcheck": [],
    "loadbalancer_algorithm": "LB_OVERRIDE",
    "endpoint_selection": "LOCAL_PREFERRED",
    "advanced_options": null
```

## Create an XC HTTP Load Balancer

Create an http load balancer with the following specs, but replace the virtual_site and pool with your information:

```json
"get_spec": {
    "domains": [
      "ollama.test"
    ],
    "http": {
      "dns_volterra_managed": false,
      "port": 80
    },
    "downstream_tls_certificate_expiration_timestamps": [],
    "advertise_custom": {
      "advertise_where": [
        {
          "virtual_site": {
            "network": "SITE_NETWORK_SERVICE",
            "virtual_site": {
              "tenant": "your-xc-tenant",
              "namespace": "c-green",
              "name": "c-green-vk8s"
            }
          },
          "use_default_port": {}
        }
      ]
    },
    "default_route_pools": [
      {
        "pool": {
          "tenant": "your-xc-tenant",
          "namespace": "c-green",
          "name": "ollama"
        },
        "weight": 1,
        "priority": 1,
        "endpoint_subsets": {}
      }
    ],
    "origin_server_subset_rule_list": null,
    "routes": [],
    "cors_policy": null,
    "disable_waf": {},
    "add_location": true,
    "no_challenge": {},
    "more_option": null,
    "user_id_client_ip": {},
    "disable_rate_limit": {},
    "malicious_user_mitigation": null,
    "waf_exclusion_rules": [],
    "data_guard_rules": [],
    "blocked_clients": [],
    "trusted_clients": [],
    "api_protection_rules": null,
    "ddos_mitigation_rules": [],
    "service_policies_from_namespace": {},
    "round_robin": {},
    "disable_trust_client_ip_headers": {},
    "disable_ddos_detection": {},
    "disable_malicious_user_detection": {},
    "disable_api_discovery": {},
    "disable_bot_defense": {},
    "disable_api_definition": {},
    "disable_ip_reputation": {},
    "disable_client_side_defense": {},
    "csrf_policy": null,
    "graphql_rules": [],
    "protected_cookies": [],
    "host_name": "",
    "dns_info": [],
    "state": "VIRTUAL_HOST_READY",
    "auto_cert_info": {
      "auto_cert_state": "AutoCertNotApplicable",
      "auto_cert_expiry": null,
      "auto_cert_subject": "",
      "auto_cert_issuer": "",
      "dns_records": [],
      "state_start_time": null
    },
    "internet_vip_info": [],
    "system_default_timeouts": {},
    "jwt_validation": null,
    "disable_threat_intelligence": {},
    "l7_ddos_action_default": {},
    "cert_state": "AutoCertNotApplicable"
  },
  ```

## Launch Jupyter Notebook

Now, open the 'ollama.ipynb' Jupyter Notebook and run all code blocks.
