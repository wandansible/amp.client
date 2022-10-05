Ansible role: AMP Client
========================

Install and configure amplet2

Role Variables
--------------

```
ENTRY POINT: main - Install and configure amplet2

OPTIONS (= is mandatory):

- amp
        List of individual amplet2 clients and their configuration
        [Default: (null)]
        elements: dict
        type: list

        OPTIONS:

        - cacert_file
            Path of the SSL CA certificate for the central collector
            [Default: /etc/amplet2/keys/<collector>.pem]
            type: str

        - cert_file
            Path of the SSL certificate for the AMP client, only used
            if waitforcert = false
            [Default: /etc/amplet2/keys/<ampname>/<collector>.cert]
            type: str

        = collector
            Hostname of the AMP collector

            type: str

        = collector_cacert
            Contents of the collector CA certificate file

            type: str

        - control_enabled
            If true, open control socket to listen for control
            messages from other amp clients to request test servers be
            started (i.e. throughput, udpstream), or to remotely run
            tests from a client. This per client value overrides the
            default value set by amp_control_enabled.
            [Default: False]
            type: bool

        - control_server_acl
            List of access rules to restrict access to the control
            socket. All rules default to deny all, with more specific
            rules overriding less specific rules. You can specify
            names in an "allow" or "deny" rule that will be matched to
            the common name in the certificate of any connecting
            client (e.g. foo.amp.example.com), or specify partial
            names by prepending the name with a dot (e.g.
            .amp.example.com will match any name ending in
            .amp.example.com).
            [Default: (null)]
            elements: str
            type: list

        - fetch_schedule_enabled
            If true, query remote server for schedule updates. This
            per client value overrides the default value set by
            amp_fetch_schedule_enabled.
            [Default: True]
            type: bool

        - fetch_schedule_frequency
            How frequently (in seconds) the client should poll for a
            new schedule file. This per client value overrides the
            default value set by amp_fetch_schedule_frequency.
            [Default: 3600]
            type: int

        - fetch_schedule_url
            Base URL for the up to date schedule file, which will
            later have the ampname appended
            [Default: https://<collector>/yaml/]
            type: str

        - interface
            Name of the source interface that all tests should bind to
            when sending test data. If not set then tests will use
            whichever interface is appropriate based on the host
            operating system, routing tables, etc.
            [Default: (null)]
            type: str

        - ipv4
            Source IPv4 address that all tests should bind to when
            sending test data. If not set then tests will use
            whichever address is appropriate based on the interface
            they are using, operating system, routing tables, etc.
            [Default: (null)]
            type: str

        - ipv6
            Source IPv6 address that all tests should bind to when
            sending test data. If not set then tests will use
            whichever address is appropriate based on the interface
            they are using, operating system, routing tables, etc.
            [Default: (null)]
            type: str

        - key_file
            Path of the SSL private key for the AMP client, only used
            if waitforcert = false
            [Default: /etc/amplet2/keys/<ampname>/key.pem]
            type: str

        = name
            Name of the amplet. This name is reported as the source of
            the tests. If SSL connections are being used, then this
            name must match the Common Name in the certificate.

            type: str

        - nameservers
            Override the default system nameservers in
            /etc/resolv.conf. If you specify more than 3, only the
            first 3 valid addresses will be used.
            [Default: (null)]
            elements: str
            type: list

        - nametable
            List of entries for the amp client's nametable. The
            nametable maps names to IP addresses, similar in approach
            to /etc/hosts. This allows the use of names to refer to
            test targets without involving DNS lookups. Each entry
            should have the format: "<ip> <domain_name>".
            [Default: (null)]
            elements: str
            type: list

        - schedule_groups
            List of groups to add to the ansible-managed schedule
            [Default: (null)]
            elements: dict
            type: list

            OPTIONS:

            = hosts
                List of hosts to add to the group

                elements: str
                type: list

            = name
                Name of the group

                type: str

        - schedule_tests
            List of tests to add to the ansible-managed schedule
            [Default: (null)]
            elements: dict
            type: list

            OPTIONS:

            = args
                Command line arguments for the test

                type: str

            = end
                Time in seconds after the start of the repeat period
                that the test should stop being scheduled

                type: int

            = frequency
                Frequency in seconds at which the test should be
                repeated within the repeat period. A value of zero
                means to run the test only once within the period.

                type: int

            = period
                Frequency at which the schedule should be repeated. A
                day starts at 00:00:00 UTC, and a week starts at
                00:00:00 Sunday UTC.
                (Choices: hour, day, week)
                type: str

            = start
                Time in seconds after the start of the repeat period
                that the first run of this test should be scheduled

                type: int

            - target
                The name/address of the destination to test to, the
                alias of a target group (group names need to be
                prefixed by a *, e.g. *<group>), or a list of any
                combination of these. If a name is given and it exists
                in the AMP nametable then the address in that file
                will be used, otherwise the name will be resolved
                using DNS.
                By default all the resolved addresses will be tested
                to (though some tests will only accept one address),
                unless there are qualifiers added, e.g.:
                       www.example.org - test to all resolved
                addresses for the site
                       www.example.org!1 - test to a single resolved
                address for the site
                       www.example.org!v4 - test to all resolved IPv4
                addresses for the site
                       www.example.org!1!v4 - test to one resolved
                IPv4 addresses for the site
                       www.example.org!v6 - test to all resolved IPv6
                addresses for the site
                       www.example.org!1!v6 - test to one resolved
                IPv6 addresses for the site
                [Default: (null)]
                elements: str
                type: list

            = test
                Name of the test to run. Examples of tests currently
                included in AMP are: icmp, traceroute, dns, http,
                throughput, tcpping, udpstream, youtube.

                type: str

        - waitforcert
            If true, wait for certificate to be signed by amppki
            server before the amp client starts up. This per client
            value overrides the default value set by amp_waitforcert.
            [Default: True]
            type: bool

- amp_apt_key_fingerprint
        Fingerprint for amp apt signing key
        [Default: (null)]
        type: str

- amp_control_enabled
        If true, open control socket to listen for control messages
        from other amp clients to request test servers be started
        (i.e. throughput, udpstream), or to remotely run tests from a
        client.
        [Default: False]
        type: bool

- amp_fetch_schedule_enabled
        If true, query remote server for schedule updates
        [Default: True]
        type: bool

- amp_fetch_schedule_frequency
        How frequently (in seconds) the client should poll for a new
        schedule file
        [Default: 3600]
        type: int

- amp_local_rabbitmq_address
        Address for rabbitmq broker running locally on the host
        [Default: (null)]
        type: str

- amp_log_level
        Log level
        (Choices: emerg, alert, crit, err, warn, notice, info,
        debug)[Default: info]
        type: str

- amp_waitforcert
        If true, wait for certificate to be signed by amppki server
        before the amp client starts up
        [Default: True]
        type: bool

- bintray_amp_apt_key_fingerprint
        Fingerprint for apt signing key for old bintray amp repo
        [Default: (null)]
        type: str

- old_amp_apt_key_fingerprint
        Fingerprint for apt signing key for old amp repo
        [Default: (null)]
        type: str
```

Installation
------------

This role can either be installed manually with the ansible-galaxy CLI tool:

    ansible-galaxy install git+https://github.com/wandansible/amp.client,main,wandansible.amp.client
     
Or, by adding the following to `requirements.yml`:

    - name: wandansible.amp.client
      src: https://github.com/wandansible/amp.client

Roles listed in `requirements.yml` can be installed with the following ansible-galaxy command:

    ansible-galaxy install -r requirements.yml

Example Playbook
----------------

    - hosts: amplets
      roles:
         - role: wandansible.amp.client
           become: true
           vars:
             amp_control_enabled: true
             amp_control_server_acl:
               - ".example.org"
             
             amp_collector: "amp.example.org"
             amp_collector_cacert: |
               -----BEGIN CERTIFICATE-----
               XXXXXXXXXXXXXXXXXXXXXXXXXXX
               -----END CERTIFICATE-----
             
             amp:
               - name: "{{ ampname | default(inventory_hostname) }}"
                 collector: "{{ amp_collector }}"
                 collector_cacert: "{{ amp_collector_cacert }}"
                 control_enabled: "{{ amp_control_enabled }}"
                 control_server_acl: "{{ amp_control_server_acl }}"
