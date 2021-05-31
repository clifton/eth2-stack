# ETH2 stack

geth + lighthouse + metrics

## Usage

1. enable grafana/prometheus `dc up -d grafana prometheus`
2. sync eth1 client `dc up -d geth`
3. sync eth2 beacon `dc up -d beacon`
4. generate validator keys and put in `./validator_keys`
5. import validator keys: `dc run --rm lighthouse-base lighthouse --network mainnet account validator import --directory /root/validator_keys`
6. start validator `dc up -d validator`

## Migrating validator to a new machine

See https://lighthouse-book.sigmaprime.io/slashing-protection.html

## Metrics (forked)

[![metrics.png](https://i.postimg.cc/Jh7rxtgp/metrics.png)](https://postimg.cc/4YMRN4Xc)

Provides a `docker-compose` environment which scrapes metrics from Lighthouse
nodes using Prometheus and presents them in a browser-based Grafana GUI.

### Metrics Usage

1. Start a lighthouse node with `$ lighthouse beacon --metrics`
    - The `--metrics` flag is required for metrics.
1. Bring the environment up with `$ docker-compose up --build`.
1. Ensure that Prometheus can access your Lighthouse node by ensuring it is in
   the `UP` state at [http://localhost:9090/targets](http://localhost:9090/targets).
1. Browse to [http://localhost:3000](http://localhost:3000)
    - Username: `admin`
    - Password: `changeme`
1. Import some dashboards from the `dashboards` directory in this repo:
    - In the Grafana UI, go to `Dashboards` -> `Manage` -> `Import` -> `Upload .json file`.
    - The `Summary.json` dashboard is a good place to start.

### Hosting Publicly

By default Prometheus and Grafana will only bind to localhost (127.0.0.1), in
order to protect you from accidentally exposing them to the public internet. If
you would like to change this you must edit the `http_addr` in `grafana.ini`.

### Maintenance and Contributing

The Lighthouse team has a hosted version of this stack where we do the
majority of the monitoring. The dashboards in this repo are not guaranteed to
be kept updated as we add/modify metrics to Lighthouse. If you're having
problems, please reach out and we can update the dashboards here.

Feel free to create your own dashboards, export them and submit them here as
PRs.
