Spyfish Aotearoa toolkit

This is my copy of the [Spyfish Aotearoa toolkit](https://github.com/wildlifeai/spyfish-aotearoa-toolkit). The original project and its contributors keep credit for their work.

The toolkit helps study fish in underwater videos from New Zealand marine reserves. It checks camera records, finds fish with a machine learning model, collects labels from volunteers and experts, and shows the results. The project is still being developed.

How it works

Rangers place baited cameras in the water and upload videos to S3. The toolkit checks the camera records and saves valid records in a SQLite database.

A YOLO model scans each video for fish and estimates the largest number of each species visible at once, called MaxN.

Volunteers label short clips on [Zooniverse](https://www.zooniverse.org/projects/victorav/spyfish-aotearoa). Marine experts review selected images on [BIIGLE](https://biigle.de). The project uses the expert labels as its reference for checking results and training the model again.

Each step keeps its own progress record. The dashboard shows the results and helps the team prepare reports.

Run it

```bash
pip install -r requirements.txt
pip install -e .

# Keep AWS, BIIGLE and Zooniverse credentials in .env, never in Git.
# See design_doc.md for the settings you need.
python run_pipeline.py --ping
python run_pipeline.py
python run_pipeline.py --ml
streamlit run "app/🐟_Spyfish_Data_Tools.py"
```

Run `pytest tests/` to check the code. Unit tests do not need outside services. Integration tests need FFmpeg.

The original project uses New Zealand's NeSI computing service for scheduled runs. `run_pipeline.sl` runs the video processing steps. `spyfish/ml/training/train_job.sl` trains the model. Both use SLURM. The same Python commands can also run on a local computer with the required files, tools and service access.

Main files

`spyfish/` contains the code that reads videos, manages records and calls the other services.

`app/` contains the dashboard. `run_pipeline.py` starts the processing steps. `config.yaml` holds the settings. `tests/` contains checks for the code. `scripts/` contains support tools.

More information

Read [the design guide](design_doc.md) for settings, commands and how the parts work together. See [the system guide](ARCHITECTURE.md) for diagrams and [the project wiki](https://spyfish.notion.site/overview) for tutorials.

The [project website](https://wildlife.ai/projects/spyfish-aotearoa/) has news and updates. Keep the original source credits and license notices when sharing this work.
