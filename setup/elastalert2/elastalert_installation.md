# Installation ElastAlert2

## Install Dependencies
```bash
sudo apt update
sudo apt install python3-pip python3-venv git -y
```

## Create a Virtual Environment
```bash
python3 -m venv elastalert2-env
source elastalert2-env/bin/activate. #inside elastalert2 root directory
```

## Clone ElastAlert2 Repository
```bash
git clone https://github.com/jertel/elastalert2.git
cd elastalert2
```

## Install ElastAlert2
```bash
pip install ".[dev]"
```

### create config file
**find it in config section of this repo**

### create first rule
**find it in directory ElastAlert Rule**

### run elastalert
```bash
python3 -m elastalert.elastalert --config config.yaml
```
