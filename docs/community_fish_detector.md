# Use the Community Fish Detector for a local run

The Community Fish Detector (CFD) is a public model that detects one class:
`fish`. It is useful for testing the pipeline with a volunteer setup. It does
not identify fish species and its results are not production species counts.

## Download the YOLO model

The CFD project now recommends RF-DETR models. This pipeline loads YOLO `.pt`
weights, so use the older [YOLOv12x CFD release](https://github.com/filippovarini/community-fish-detector/releases/tag/cfd-1.00-yolov12x).
Download its `.pt` asset. Do not use the newer `.pth` RF-DETR assets with this
pipeline.

The release notes identify the checkpoint's original training stamp as
`20250711_193705`. Copy the downloaded weights into the configured pipeline
model directory and give the file the standard model name:

```bash
mkdir -p process_files/models/pipeline_model
cp /path/to/downloaded/model.pt process_files/models/pipeline_model/species_20250711_193705.pt
```

Keep only one model file in `process_files/models/pipeline_model`. The
pipeline takes the model name from the filename and stores it with the
detections. If you change the model path in `config.yaml`, put the file in
that configured directory instead.

The CFD release was trained and released with a 1024-pixel input size. Set
`ml_inference.imgsz` to `1024` in your local `config.yaml` when running this
model. Keep local settings and downloaded weights out of commits.

## What to expect

The CFD model returns the class `fish` for every detection. The pipeline can
use these detections to check video loading, inference, and frame selection,
but it cannot produce species-level results from this model. Do not use these
results for reporting or compare them with production species counts.

The model weights are maintained by the CFD contributors. The YOLOv12x
release uses the AGPL inference license. The CFD project notes that the
training datasets have different licenses; check its [license and dataset
notes](https://github.com/filippovarini/community-fish-detector#models) before
redistributing the weights or using them outside a local test.
