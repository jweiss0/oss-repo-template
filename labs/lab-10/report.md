## Checkpoint 1
Plot:

## Checkpoint 2
Modified code:
```
num_rows = 5
num_cols = 3
num_images = num_rows*num_cols
plt.figure(figsize=(2*2*num_cols, 2*num_rows))
for i in range(num_images):
  plt.subplot(num_rows, 2*num_cols, 2*i+1)
  plot_image(i+9000, predictions[i+9000], test_labels, test_images)
  plt.subplot(num_rows, 2*num_cols, 2*i+2)
  plot_value_array(i+9000, predictions[i+9000], test_labels)
plt.tight_layout()
plt.show()
```

Plot:

## Checkpoint 3
#### Image 1
```
[[6.1180799e-05 2.1125488e-06 4.3054075e-05 4.4191934e-04 2.9463494e-05
  1.1977163e-02 1.1360418e-04 9.8588526e-01 4.6821393e-04 9.7802212e-04]] 7 Sneaker
```

#### Image 2
```
[[2.5792466e-03 7.6301665e-05 1.7798821e-03 5.4101138e-03 9.5045763e-01
  1.1677409e-08 8.6682048e-05 2.0947148e-09 3.9610170e-02 4.6175930e-10]] 4 Coat
```

#### Image 3
```
[[8.7929621e-02 3.8250483e-04 9.5286317e-02 1.0636465e-02 1.7029420e-01
  3.3843510e-06 2.1539435e-03 1.4210461e-07 6.3331115e-01 2.1743499e-06]] 8 Bag
```
