## Python

**Read Data from Folder**
```
from google.colab import drive
drive.mount('/content/gdrive')
import pandas as pd
data = pd.read_csv('/content/gdrive/My Drive/calories (1).csv')
```

**Check Column Types**
