---
layout: post
title:      "Pytorch Image Segmentation Tutorial For Beginners - II"
date:       2020-08-04 21:18:45 +0000
permalink:  pytorch_image_segmentation_tutorial_for_beginners_-_ii
---

### Making masks for Brain Tumor MRI Images in Pytorch

In the previous tutorial, we prepared data for training. We downloaded the dataset, loaded the images, split the data, defined model structure, downloaded weights, defined training parameters.

In the second part of the tutorial, we train the model and evaluate the results of the model. 
Train the model 


# Move the model to GPU
model.to(device)
# Initialize lists to store loss values
loss_history = []
loss_history_val = []
best_loss_val = float('inf')
# Train
print("Start train…")
for epoch in range(50):
#Train mode
model.train()
loss_running = []
for _, (x,y) in enumerate(train_dataloader):
x, y = x.float().to(device), y.float().to(device)
pred = model(x)
loss = criterion(pred, y)
loss_running.append(loss.item())
optimizer.zero_grad()
loss.backward()
optimizer.step()
loss_history.append(np.mean(loss_running))
# Evaluate mode
model.eval()
with torch.no_grad():
loss_val_running = []
for _, (x_val, y_val) in enumerate(val_dataloader):
x_val, y_val = x_val.to(device), y_val.to(device)
pred_val = model.forward(x_val) #pred_val = model(x_val)
loss_val= criterion(pred_val, y_val)
loss_val_running.append(loss_val.item())
curr_loss_val = np.mean(loss_val_running)
loss_history_val.append(curr_loss_val)
# Save the best weights
if curr_loss_val < best_loss_val:
best_loss_val = curr_loss_val
torch.save(model.state_dict(), '/content/drive/My Drive/Brain-Tumor-Segmentation-Project/best_model.pth')
# Change the learning rate
scheduler.step()
# Print the results
print("epoch", epoch, "train loss", loss_history[-1], "val loss", loss_history_val[-1])
