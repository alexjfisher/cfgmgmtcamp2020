<!SLIDE >
# Too much data

## common.yaml

* What should you put in common.yaml?

<!SLIDE >
# Too much data - This is too much!
    @@@ Console
    [root@puppet ~]# wc -l /etc/puppet/hieradata/common.yaml 
    1765 /etc/puppet/hieradata/common.yaml

<!SLIDE >
# Too much data - Reams of 'stuff'
    @@@ Yaml
    system_properties::config:
      global:
        java.net.preferIPv4Stack:                "true"
        healthChecks.warnTimeoutTimeoutMS:       10000
        org.jboss.as.logging.per-deployment:     "false"
        org.apache.cxf.Logger:                   "org.apache.cxf.common.logging.Slf4jLogger"
        health.checker.timeout:                  5
        config.path.cache.ttl:                   3600000
        config.path.cache.manager.jndi:          "java:jboss/infinispan/container/clusteredCacheContainer"
      PortalAMF:
        streamer.topic.name:                     "jms_net_blue_topic_TM_TS_APP"
        ps.persistence.refdata.jndi.name:        "jdbc/com.redacted.BluePortal.RefData"
        portal.feedback.email:                   "REDACTED"
        ps.portalServer.socket.clamScan.host:    "localhost"
        ps.portalServer.socket.clamScan.port:    "3310"
        ps.portalServer.socket.clamScan.timeout: 5000
        portal.mail.smtp.from:                   "do-not-reply@REDACTED.com"
        portal.mail.smtp.host:                   "4.4.4.4"
    *    portal.mail.smtp.port:                   25
        cm.security.sessionTimeout:              1800000
        streamer.search.queryresults.max:        5000
      batch-transaction-processor:
        batch.processor.mail.smtp.from:          "do-not-reply@REDACTED.com"
        batch.processor.mail.smtp.host:          "REDACTED"
      etc: ...

<!SLIDE bullets incremental>
# What *does* go in common.yaml?

* **Not** things that aren't common!
* **Not** things that will never change.
* Do you *ever* plan on sending mail on something other than port 25?
* Your profiles are also your data. Don't be afraid to hardcode values.
* Make use of a hierarchy, but don't over do it either.

<!SLIDE bullets incremental>
# Good hierarchy parameters

* fqdn
* role
* tier
* location
* application ?


<!SLIDE bullets incremental>
# Is that enough?

    @@@ Yaml
    hierarchy:
      - name: 'Per-node data'
        path: "nodes/%{::fqdn}.yaml"
      - name: 'Location and Environment specific role data'
        path: "roles/%{::role}/environments/%{::tier}/locations/%{::location}.yaml"
      - name: 'Environment specific role data'
        path: "roles/%{::role}/environments/%{::tier}.yaml"
      - name: 'Role data'
        path: "roles/%{::role}/role.yaml"
      - name: 'Location and Environment specific application shared data'
        path: "applications/%{::application}/environments/%{::tier}/locations/%{::location}.yaml"
      - name: 'Environment specific application shared data'
        path: "applications/%{::application}/environments/%{::tier}.yaml"
      - name: 'Application shared data'
        path: "applications/%{::application}/application.yaml"
      - name: 'Default Environment specific data'
        path: "environments/%{::tier}.yaml"
      - name: 'Default data'
        path: "common.yaml"

* yeah, probably.  A fleshed out config with just a few parameters is already quite long.
