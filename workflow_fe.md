# The Xing Work Flow - Frontend

This is a general guide to creating a feature in Xing in the frontend.

In this example, we will be creating feature that allows a user to read or write Dogs to correspond with the API resource for Dogs we built in "The Xing Work Flow - Backend".

Some things to consider:
* The frontend is its own MVC situation. We will have to set up a frontend resource to consume the backend API.
* We have a divergence of code bases for Xing between MindSwarms and Yoric. So we will be describing the old style (MSW) and new style (Yoric) for Xing.

## Setting up the Frontend resource