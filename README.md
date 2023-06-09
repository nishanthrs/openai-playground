# openai-playground

### Authentication:
Set via env vars in python3 venv:
```
export OPENAI_API_KEY="<openapi_secret_key>"
```

### Install Dependencies:
```bash
source env/bin/activate  # Activate virtual env
pip3 install <dependency_name>
deactivate
```

Dependencies:
```bash
ffmpeg-python
whispercpp
numpy
openai
```
NOTE: [`whispercpp` Python bindings lib](https://github.com/aarnphm/whispercpp)
Use `pip install git+https://github.com/aarnphm/whispercpp.git -vv` to install the latest version.

### Run Script:
```bash
source env/bin/activate  # Activate virtual env
python3 open_ai_embeddings_gen.py
```

### Using yt-dlp Audio on Mac M1:
```bash
# Download Youtube video with highest quality as .wav file
yt-dlp --ffmpeg-location ffmpeg  --extract-audio --audio-format wav --audio-quality 0 "<youtube_url>"
# Convert to 16 khz (whisper.cpp only works on 16-bit wav files)
ffmpeg -i <input_name>.wav -ar 16000 -ac 1 -c:a pcm_s16le <output_name>.wav
```
[Docs](https://github.com/yt-dlp/yt-dlp#usage-and-options)
NOTE: Add ffmpeg to path:
```
sudo cp ./ffmpeg /usr/local/bin
sudo chmod ugo+x /usr/local/bin/ffmpeg
export PATH="/usr/local/bin:$PATH"
# Restart terminal
which ffmpeg
```

### Using whisper.cpp on Mac M1:
```bash
bash ./models/download-ggml-model.sh <model_name>
make
./main -m models/ggml-<model_name>.bin -f ../sample_audios/output.wav
```

[List of Whisper Models in GGML Format](https://huggingface.co/ggerganov/whisper.cpp)

### Typesense
* Followed [installation docs](https://typesense.org/docs/guide/install-typesense.html#mac-via-homebrew)
* Start typesense server:
```
./typesense-server --data-dir=$(pwd)/typesense-data --api-key=$TYPESENSE_API_KEY --enable-cors
```
* Install Python client: `pip3 install typesense`

### Performance
* Video download and processing time: ~0.8 - 1 sec
* Transcription of 13:28 video:
    * Python API (num_procs=4): 1286.38 seconds (over 20 mins!)
    * Python API (num_procs=1): 942.03 seconds
    * Bash command: 56.07 seconds
* Why tf is the python API for whisper.cpp so much slower than the raw bash command??
