### Finding the top 10 largest facts

Run the following on the box with puppetDB running:

```
su - pe-postgres -s '/bin/bash' -c '/opt/puppetlabs/server/bin/psql -d pe-puppetdb -c select certname,value,name,char_length(value) from facts inner join fact_values on facts.fact_value_id=fact_values.id inner join fact_paths on fact_paths.id=facts.fact_path_id inner join factsets on facts.factset_id=factsets.id where value is not null order by char_length(value) desc limit 10) to '/tmp/output.csv' WITH CSV HEADER;
```
