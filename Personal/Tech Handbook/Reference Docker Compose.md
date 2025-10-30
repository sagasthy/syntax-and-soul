# Reference Docker Compose

```
#---#
#docker-compose-base.yml

version: '2.1'
services:
  settlementonboardserv:
    image: dockerhub.paypalcorp.com/altus-ci/settlementonboardserv:1.0.0_20210322162447963
    container_name: settlementonboardserv_1.0.0_20210322162447963
    stdin_open: true
    network_mode: "host"
    depends_on:
      keymakeragent:
        condition: service_started

      pplocale:
        condition: service_started

      wowo:
        condition: service_started

#EVERGREEN: Added automatically by Evergreen. DO NOT REMOVE THIS COMMENT NOR THE LINE(S) IT REFERS TO, otherwise main application and future Evergreen updates will NOT be functional.
      appidentityfetcher:
        condition: service_started
    user: cronusapp
    volumes:
      - ./log/applogs/:/applicationpackages/manifests/active/settlementonboardserv/cronus/scripts/app/logs/
      - ./log/deploylogs/:/applicationpackages/manifests/active/settlementonboardserv/cronus/scripts/log/
      - ./appdata:/appdata
      - /etc/syshiera.yaml:/etc/syshiera.yaml
      - keymakeragent-identities:/x/web/keymaker/identities/
      - keymaker-certs:/x/web/LIVE/keymaker/certs
      - settlementonboardserv:/data
      - settlementonboardserv:/applicationpackages/.appdata

      - keystore:/applicationpackages/manifests/active/sharedkeystore_unifiedjava

      - pplocale:/x/web/LIVE/locale

      - wowo:/x/web/LIVE/wowo

    environment:
      - CRONUSAPP_HOME=/applicationpackages
      - TERM=xterm
      - AUTHCODE
      - AUTHCODE2
      - METADATA_ENV_FILE
      - METADATA_JSON_FILE
      - ISDOCKER=true
      - ISUBUNTU=true
      - REPOSITORY=settlementonboardserv
      - BOOTSTRAP_TOKEN
      - bootstrap_token

      - WAITFOR_PPLOCALE=true

      - WAITFOR_WOWO=true

#EVERGREEN: Added automatically by Evergreen. DO NOT REMOVE THIS COMMENT NOR THE LINE(S) IT REFERS TO, otherwise main application and future Evergreen updates will NOT be functional.
      - EVERGREEN_MANIFEST_ID=settlementonboardserv-1.0.0_20210322162447963
#EVERGREEN: Added automatically by Evergreen. DO NOT REMOVE THIS COMMENT NOR THE LINE(S) IT REFERS TO, otherwise main application and future Evergreen updates will NOT be functional. This environment variable was added by service evergreen_jvm_infra
      - INFRA_BUNDLE_DIR=/evergreen/jvm-infra
#EVERGREEN: Added automatically by Evergreen. DO NOT REMOVE THIS COMMENT NOR THE LINE(S) IT REFERS TO, otherwise main application and future Evergreen updates will NOT be functional. This environment variable was added by service evergreen_jvm_infra
      - JAVA_HOME=/evergreen/jvm-infra/java
    labels:
      primary.container: "true"
    volumes_from:
#EVERGREEN: Added automatically by Evergreen. DO NOT REMOVE THIS COMMENT NOR THE LINE(S) IT REFERS TO, otherwise main application and future Evergreen updates will NOT be functional.
      - evergreen_jvm_infra:ro
  keymakeragent:
    image: dockerhub.paypalcorp.com/paas/keymakeragent:214.0.54933849.latest
    container_name: keymakeragent_1.0.0_20210322162447963
    network_mode: "host"
    stdin_open: true
    restart: "on-failure:3"
    volumes_from:
      - keymakeragentprotected
    volumes:
      - /etc/syshiera.yaml:/etc/syshiera.yaml
      - keymakeragent-identities:/x/web/keymaker/identities
      - keymaker-certs:/x/web/LIVE/keymaker/certs
    environment:
      - AUTHCODE
      - AUTHCODE2

  pplocale:
    image: dockerhub.paypalcorp.com/paas/pplocale:214.0.55452164
    container_name: pplocale_1.0.0_20210322162447963
    volumes:
      - /etc/syshiera.yaml:/etc/syshiera.yaml
      - pplocale:/x/web/LIVE/locale

  wowo:
    image: dockerhub.paypalcorp.com/paas/wowo:214.0.55452164
    container_name: wowo_1.0.0_20210322162447963
    volumes:
      - /etc/syshiera.yaml:/etc/syshiera.yaml
      - wowo:/x/web/LIVE/wowo

  keymakeragentprotected:
    container_name: keymakeragentprotected_1.0.0_20210322162447963
    image: dockerhub.paypalcorp.com/keystore/keymakeragent_keystore:stage_latest
    volumes:
      - /etc/syshiera.yaml:/etc/syshiera.yaml

  keystorecontainer:
    image: dockerhub.paypalcorp.com/keystore/sharedkeystore_unifiedjava:qa_1.0.60089
    container_name: keystorecontainer_1.0.0_20210322162447963
    volumes:
      - keystore:/keystorevolume

#EVERGREEN: Added automatically by Evergreen. DO NOT REMOVE THIS COMMENT NOR THE LINE(S) IT REFERS TO, otherwise main application and future Evergreen updates will NOT be functional.
  appidentityfetcher:
    image: dockerhub.paypalcorp.com/paas/appidentity-fetcher:1.12-030121
    container_name: appidentityfetcher_1.0.0_20210322162447963
    network_mode: "host"
    volumes:
      - /etc/syshiera.yaml:/etc/syshiera.yaml
      - keymakeragent-identities:/x/web/identities/
      - keymaker-certs:/x/web/LIVE/keymaker/certs
    environment:
      - APPNAME
#EVERGREEN: Added automatically by Evergreen. DO NOT REMOVE THIS COMMENT NOR THE LINE(S) IT REFERS TO, otherwise main application and future Evergreen updates will NOT be functional.
  evergreen_jvm_infra:
    image: dockerhub.paypalcorp.com/addons/evergreen_jvm_infra:3.11.30_zulu1.8.0_232_v1
    container_name: evergreen_jvm_infra_1.0.0_20210322162447963
volumes:
  settlementonboardserv:
    driver: local
  keymakeragent-identities:
    driver: local
  keymaker-certs:
    driver: local

  keystore:
    driver: local

  pplocale:
    driver: local

  wowo:
    driver: local

#---#
#docker-compose-qa.yml

version: '2.1'

#Jacoco data volume
volumes:
  jacoco:
    driver: local
  iast:
    driver: local

services:

  #Application overrides
  settlementonboardserv:
    volumes:
     - jacoco:/x/web/jacoco
     - /applicationpackages
     - /x/web/LIVE
    volumes_from:
     - predeployhooks
     - iast
    environment:
     - HAPROXY_JSON

  #Jacoco data container
  jacococontainer:
    image: dockerhub.paypalcorp.com/addons/jacoco:0.0.2
    container_name: jacoco_1.0.0_20210322162447963
    volumes:
     - jacoco:/x/web/jacoco

  # Cyclopsd application log analysis container
  cyclopsdcontainer:
    image: dockerhub.paypalcorp.com/addons/cyclopsd:1.0.25-1
    network_mode: "host"
    container_name: cyclopsd_1.0.0_20210322162447963
    volumes_from:
      - settlementonboardserv

  predeployhooks:
    image: dockerhub.paypalcorp.com/paas/paas-hooks:1.15.23
    container_name: predeployhooks_1.0.0_20210322162447963
    network_mode: "host"

  #Interactive Application Security Testing(IAST) data container
  iast:
    image: dockerhub.paypalcorp.com/application-security/iast_agent:latest
    container_name: iast_1.0.0_20210322162447963
    volumes:
      - iast:/x/web/iast/

#---#
#docker-compose-qa-testenv.yml

version: '2.1'

services:
  #Application overrides
  settlementonboardserv:
    volumes_from:
     - testenvconfigurator

  testenvconfigurator:
    image: dockerhub.paypalcorp.com/developerexperience-r/testenv-configurator:v0.2.0
    container_name: testenvconfigurator_1.0.0_20210322162447963
    network_mode: "host"

#---#
#docker-compose-production.yml

version: '2.1'
services:

  #Application overrides
  settlementonboardserv:
    volumes:
     - /applicationpackages/manifests/active/settlementonboardserv/cronus/scripts/
     - /x/web/LIVE/

  #Keymaker Protected Packages Container
  keymakeragentprotected:
    image: dockerhub.paypalcorp.com/keystore/keymakeragent_keystore:production_latest

  #Application Protected Packages Container Overrides
  keystorecontainer:
    image: dockerhub.paypalcorp.com/keystore/sharedkeystore_unifiedjava:production_1.0.60089

  # Diagnosticsagent container
  diagnosticsagentcontainer:
    image: dockerhub.paypalcorp.com/addons/diagnosticsagent:213.0-20180223.19
    container_name: diagnosticsagent_1.0.0_20210322162447963
    network_mode: "host"
    stdin_open: true
    restart: "on-failure:3"
    # expose logs and manifest.json to diagnostics agent
    volumes:
     - ./log/applogs/:/x/web/LIVE/settlementonboardserv/logs/
     - ./appdata/:/x/web/LIVE/settlementonboardserv/

#---#
#docker-compose-sandbox.yml

version: '2.1'
services:

  #Keymaker Protected Packages Container
  keymakeragentprotected:
    image: dockerhub.paypalcorp.com/keystore/keymakeragent_keystore:sandbox_latest
Docker
  #Application Protected Packages Container Overrides
  keystorecontainer:
    image: dockerhub.paypalcorp.com/keystore/sharedkeystore_unifiedjava:sandbox_1.0.60089

#---#
#docker-compose-debug.yml

version: '2.1'
services:

  settlementonboardserv:
    entrypoint: /bin/bash -c "sleep infinity"

#---#
#docker-compose-healthcheck.yml

version: '2.1'
services:

  settlementonboardserv:
    healthcheck:
      interval: 10s
      timeout: 5s
      retries: 100
      test: "/bin/bash /healthcheck.sh"
    labels:
      healthcheck.container: "true"

```