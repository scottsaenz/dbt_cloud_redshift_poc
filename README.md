# dbt Cloud Redshift POC

This is a proof-of-concept (POC) project for connecting dbt Cloud to Amazon Redshift.

## Overview

This project demonstrates how to set up and configure a dbt project to work with Amazon Redshift as the data warehouse. It includes:

- dbt project configuration
- Example models and transformations
- Redshift-specific connection settings
- Best practices for dbt Cloud integration

## Prerequisites

- Amazon Redshift cluster (running and accessible)
- dbt Cloud account or local dbt installation
- Python 3.7+ (for local development)

## Project Structure

```
dbt_cloud_redshift_poc/
├── models/              # dbt models (SQL transformations)
│   └── example/         # Example models
├── analyses/            # Ad-hoc queries
├── tests/               # Custom data tests
├── seeds/               # CSV files to load into warehouse
├── macros/              # Reusable SQL macros
├── snapshots/           # Type-2 slowly changing dimensions
├── dbt_project.yml      # dbt project configuration
├── profiles.yml.example # Connection profile template
└── requirements.txt     # Python dependencies
```

## Setup Instructions

### For dbt Cloud

1. **Create a new project in dbt Cloud**
   - Navigate to your dbt Cloud account
   - Create a new project and connect it to this repository

2. **Configure Redshift Connection**
   - In dbt Cloud, go to Settings → Connections
   - Select "Redshift" as the database type
   - Enter your connection details:
     - **Host**: Your Redshift cluster endpoint (e.g., `your-cluster.region.redshift.amazonaws.com`)
     - **Port**: 5439 (default Redshift port)
     - **Database**: Your database name
     - **Schema**: Target schema (e.g., `public` or `analytics`)
     - **User**: Redshift username
     - **Password**: Redshift password

3. **Set Environment Variables (Recommended)**
   - For production, use dbt Cloud environment variables:
     - `DBT_REDSHIFT_HOST`
     - `DBT_REDSHIFT_USER`
     - `DBT_REDSHIFT_PASSWORD`
     - `DBT_REDSHIFT_DATABASE`

4. **Test Connection**
   - Use the "Test Connection" button in dbt Cloud to verify connectivity

### For Local Development

1. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

2. **Configure profiles**
   ```bash
   # Copy the example profile
   cp profiles.yml.example ~/.dbt/profiles.yml
   
   # Edit ~/.dbt/profiles.yml with your Redshift credentials
   ```

3. **Test connection**
   ```bash
   dbt debug
   ```

4. **Run the project**
   ```bash
   # Compile models
   dbt compile
   
   # Run models
   dbt run
   
   # Test models
   dbt test
   ```

## Example Models

This project includes two example models to demonstrate dbt functionality:

- **my_first_dbt_model**: A simple model that creates sample data
- **my_second_dbt_model**: A model that references the first model

## Redshift-Specific Configuration

### Connection Parameters

The following Redshift-specific parameters are configured in the profile:

- **keepalives_idle**: Prevents connection timeouts (set to 0)
- **connect_timeout**: Connection timeout in seconds (set to 10)
- **search_path**: Schema search path for queries
- **threads**: Number of concurrent threads for model execution (set to 4)

### Materialization Strategies

dbt supports several materialization strategies with Redshift:

- **view**: Creates a Redshift view (default for this project)
- **table**: Creates a physical table
- **incremental**: Updates existing tables incrementally
- **ephemeral**: Creates CTEs (not materialized in database)

Example configuration in `dbt_project.yml`:
```yaml
models:
  dbt_cloud_redshift_poc:
    example:
      +materialized: view
```

## Running in dbt Cloud

Once configured, you can:

1. **Schedule runs**: Set up jobs to run on a schedule
2. **Monitor executions**: View run history and logs
3. **Generate documentation**: Automatically generate and host documentation
4. **Set up alerts**: Get notified about job failures

## Security Best Practices

- Never commit `profiles.yml` with credentials to version control
- Use dbt Cloud environment variables for sensitive data
- Restrict Redshift user permissions to only what's needed
- Use IAM authentication when possible
- Enable SSL connections to Redshift

## Troubleshooting

### Connection Issues

If you encounter connection problems:

1. Verify Redshift cluster is running
2. Check security group rules allow connections from your IP/dbt Cloud
3. Verify credentials are correct
4. Ensure the database and schema exist

### Common Errors

- **"could not connect to server"**: Check network/firewall settings
- **"password authentication failed"**: Verify username and password
- **"database does not exist"**: Create the database or update configuration
- **"permission denied for schema"**: Grant necessary permissions to the user

## Resources

- [dbt Documentation](https://docs.getdbt.com/)
- [dbt Cloud Documentation](https://docs.getdbt.com/docs/dbt-cloud/cloud-overview)
- [Amazon Redshift Documentation](https://docs.aws.amazon.com/redshift/)
- [dbt-redshift Adapter](https://docs.getdbt.com/reference/warehouse-setups/redshift-setup)

## Next Steps

1. Replace example models with your own transformations
2. Add sources to connect to raw data tables
3. Create custom tests for data quality
4. Set up documentation for your models
5. Configure production jobs in dbt Cloud

## License

This is a proof-of-concept project for demonstration purposes.