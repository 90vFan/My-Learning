## dot-product of Query and Key

![C4W4_LN2_image2](images/C4W4_LN2_image2.png)

![C4W4_LN2_image3](images/C4W4_LN2_image3.png)

The *attend* function receives *Query* and *Key*.

- n_seq: input words sequence length
- n_q: size of query and key

$QK^{T}$ similarity of Q and K

the resulting dot-product (*Dot*) entries describe a complete (n_seq,n_seq) map of the similarity of all entries of q vs all entries of k.



## Masking

![C4W4_LN2_image4](images/C4W4_LN2_image4.png)

to exclude results that occur later in time (causal) or to mask padding or other inputs.

## Softmax

![C4W4_LN2_image5](images/C4W4_LN2_image5.png)



$$softmax(x_i)=\frac{\exp(x_i)}{\sum_j \exp(x_j)}\tag{1}$$

## applying attention to V

![C4W4_LN2_image6](images/C4W4_LN2_image6.png)



The purpose of the dot-product is to 'focus attention' on some of the inputs. Dot now has entries appropriately scaled to enhance some values and reduce others

𝑉V is of size (n_seq,n_v)

![C4W4_LN2_image7](images/C4W4_LN2_image7.png)

$Z_{00} = W[0, :] * V[:, 0]$

