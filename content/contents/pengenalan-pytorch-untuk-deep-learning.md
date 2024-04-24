---
title: "Pengenalan PyTorch untuk Deep Learning"
date: 2018-12-21T23:47:00+08:00
publishDate: 2018-12-21T23:47:00+08:00
image: "/images/2018/pytorchudacityscholar.png"
url: "/content/pengenalan-pytorch-untuk-deep-learning"
tags: ["Udacity", "Machine Learning", "PyTorch", "course"]
draft: false
---

{{< figure src="/images/2018/pytorchudacityscholar.webp" title="Pengenalan Neural Networks" alt="Pengenalan Neural Networks" position="center" >}}


> Tulisan ini adalah catatan belajar PyTorch Udacity Scholar nd188. Semua gambar milik kelas tersebut. Kelas ini sekarang tersedia FREE sebagai [ud188](https://www.udacity.com/course/deep-learning-pytorch--ud188).

Instalasi paket **torch** dan **torchvision**, sesuaikan dengan paket manager yang digunakan [^1], misalnya **pip** atau **conda**.

Misalnya menggunakan pip, di linux ini akan mendownload paket torch sekitar 750MB:

```
pip install torch torchvision
```

Untuk mengecek apakah paketnya sudah terinstal dengan baik.

```python
from __future__ import print_function
import torch
x = torch.rand(5, 3)
print(x)
```

Untuk yang menggunakan GPU CUDA, bisa mengecek dengan cara berikut.

```python
import torch
torch.cuda.is_available()
```

Kita akan menggunakan kode yang sudah tersedia di github repo udacity [^2], di dalamnya ada direktori **intro-to-pytorch**.

Untuk menggunakan jupyter notebook (iPython Notebook), kita butuh install paket `jupyter` dan `notebook`. Atau bisa juga menggunakan google colab, tapi butuh memindahkan file atau copy-paste ke google colab.

```
pip install jupyter notebook
```

Dengan jupyter dan notebook yang sudah teristall, prosesnya cukup _run command_ berikut di direktori hasil clone repo udacity:

```
jupyter notebook
```

Ini adalah kode training pertama yang diambil dari notebook [Part 3 - Training Neural Networks (Exercises)](https://github.com/udacity/deep-learning-v2-pytorch/blob/master/intro-to-pytorch/Part%203%20-%20Training%20Neural%20Networks%20(Exercises).ipynb).

```python
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
# Assuming that we are on a CUDA machine, this should print a CUDA device:
print(device)

model = nn.Sequential(nn.Linear(784, 128),
                      nn.ReLU(),
                      nn.Linear(128, 64),
                      nn.ReLU(),
                      nn.Linear(64, 10),
                      nn.LogSoftmax(dim=1)).to(device)

criterion = nn.NLLLoss()
optimizer = optim.SGD(model.parameters(), lr=0.003)

epochs = 5
for e in range(epochs):
    running_loss = 0
    for images, labels in trainloader:
        images, labels = images.to(device), labels.to(device)

        # Flatten MNIST images into a 784 long vector
        images = images.view(images.shape[0], -1)
    
        # Clear the gradients, do this because gradients are accumulated
        optimizer.zero_grad()

        # Forward pass, then backward pass, then update weights
        output = model(images)
        loss = criterion(output, labels)
        loss.backward()
        
        # Take an update step and few the new weights
        optimizer.step()
        
        running_loss += loss.item()
    else:
        print(f"Training loss: {running_loss/len(trainloader)}")
```

Untuk `device = 'cpu'` penggunaan prosesornya hampir selalu penuh (100%) selama periode training.

{{< figure src="/images/2018/pt001.webp" title="Training Neural Network dengan CPU" alt="Training Neural Network dengan CPU" position="center" >}}

Saat dipindahkan ke GPU dengan `device = 'cuda:0'` penggunaan prosesornya hanya sesekali penuh saat periode training.

{{< figure src="/images/2018/pt002.webp" title="Training Neural Network denganCUDA GPU" alt="Training Neural Network dengan CUDA GPU" position="center" >}}



**TBD**

[^1]: https://pytorch.org/get-started/locally/

[^2]: https://github.com/udacity/deep-learning-v2-pytorch

