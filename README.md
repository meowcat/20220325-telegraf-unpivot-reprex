# 20220325-telegraf-unpivot-reprex

Steps to reproduce:

* Clone repo
* Unzip "in_mini.zip" to `data/in/", it should then contain 334 CSV files
* In the repo folder, run `docker-compose -f docker-compose-nopivot.yaml up`
  * This starts Telegraf with `telegraf-nopivot.conf` as config, where `directory_monitoring` input writes directly to `file` output
* This processes all of `data/in` correctly and moves the files to `data/out`
* When this is done, kill the docker (Ctrl-C)
* Move files from `data/out` back to `data/in` (or delete them and unzip `data.zip` again)
* In the repo folder, run `docker-compose -f up`
  * This starts Telegraf with `telegraf.conf` as config, where `directory_monitoring` input is rotated to long form with the `unpivot` processor and then written to `file` output
* This processes ~20 files (also dropping many metrics, but this is less relevant) and then completely stops. 
* Killing and restarting the docker will process another ~10-20 files and stop again; this can be repeated to process more files. So any specific input file is not the problem.
* Note that 
  * other inputs would continue to work in parallel with no problems
  * this happens with `file` and `influxdb` output plugins, as well as both in parallel.
