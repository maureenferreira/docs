# How to do automated imports

## Preparation

First, do this:

1. [How to install the Data Importer](../installation/docker.md)
2. [How to import using the CLI](../advanced/cli.md)

## Configuration

Then, in the `.importer.env`-file:

- Add `IMPORT_DIR_ALLOWLIST=/import`
- Add value for `FIREFLY_III_ACCESS_TOKEN`
- Add `NORDIGEN` or `SPECTRE` credentials

And in the Docker Compose file from the installer, mount a local directory to `/import` in the container:

```yaml
volumes:
  - ./import:/import
```

## Run the import once

In the Data Importer, run through the import and download the configuration file. Put the JSON-file in the import folder you created and bound to the container. Be sure to name it `config.json`. The name can be anything though.

## Run another import, automatically

Run the following command:

```bash
docker exec firefly_iii_importer php artisan importer:import /import/config.json
```

Where `firefly_iii_importer` is the container ID, and where `config.json` is the configuration file from the previous step. This should run the import again.

## Make it a cron job

To make this a cron job, run `crontab -e` and add the following line:

```cronexp
0 22 * * * docker exec firefly_iii_importer php artisan importer:import /import/config.json
```

*Credits go to [@ShooTeX](https://github.com/orgs/firefly-iii/discussions/8249#discussioncomment-7850858) for this how-to.*
