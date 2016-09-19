Using autoencoders to build recommender systems

Task description

We need to build recommender systems that:
1.	Can be speed optimal in the model using period. 
2.	Will be speed effective while processing new data. It will not need to rebuild the model.   

Decision

We will use the next data.
1.	The user data (i.m. for example sex, age for the MovieLens 1M dataset).
2.	The product data (i.m. for example year of film production for the MovieLens 1M dataset).
3.	The rating data (i.m. for example day of rate the film for the MovieLens 1M dataset).
4.	The rate (or rates) itself
5.	The user id vector.
6.	The product id vector.

The learn phase of model

First we need to train autoencoders. There are to autoencoders in system for one pair of the product-user. 

Let the first autoencoder  will take M (5..9) items of the next record of rating some product by some user:  1) the user id vector; 2) the user data; 3) the rate (or rates) of this user for some product and 4) the rating data (of this user for some product). All that records are for the one product at one time. And it will encode the product vector id representation. 

Let the second autoencoder will take M (5..9) items of the next record of rating some product by some user: 1) the product id vector: 2) the product data; 3) the rate (o rates) of the user for this product  and 4) the rating data (of some user  to this product). All that records are for the one user that rates some products. This autoencoder will encode the user vector id representation.

In 100..1000 cycles after we have trained autoencoders, we starts getting  products and users id vectors.

For one product we get (24..64) encoded values of running the first autoencoder. Then we get average of that encoded values and so we get new product id vector to move to. We repeat that process for some products. So we will get new products id vectors to move old id vectors to. Same we do for users. And will get new users id vectors to move old id vectors to.

But I should mention that we need to take into account the �breathing� of encoded values of autoencoders. I used getting the average offset of id vectors to correct such �breathing�. See code for details.

Then we repeat training of autoencoders for corrected id values. And then get new id values once again. And so on. 

So we get products and users id vectors. That id vectors will be used in prediction of rating value(s). We train neural net that takes 1) user id vector; 2) user data; 3) product id vector; 4) product data; 5) rating data to produce rating of the product by the user.

After building the model we will use it.
1.	We predict ratings of the product by the user.
2.	We correct id values after getting new ratings from users.
3.	We add new id vectors of new products and users. 

When we use model we do not train neural nets, we just compute the id vector as we did in the train model phase.




