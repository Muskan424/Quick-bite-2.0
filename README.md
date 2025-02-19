// Full Backend for Food Delivery App 
// Technologies: Node.js, Express, MongoDB, EJS

const express = require("express");
const mongoose = require("mongoose");
const bodyParser = require("body-parser");
const path = require("path");

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(bodyParser.urlencoded({ extended: true }));
app.use(express.static("public")); // For CSS, images, etc.
app.set("view engine", "ejs");

// MongoDB Connection
mongoose.connect("mongodb://localhost:27017/foodDelivery", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
}).then(() => console.log("MongoDB Connected"))
  .catch(err => console.log(err));

// Food Item Schema
const FoodSchema = new mongoose.Schema({
    name: String,
    price: Number,
    image: String,
});
const Food = mongoose.model("Food", FoodSchema);

// Order Schema
const OrderSchema = new mongoose.Schema({
    customerName: String,
    customerEmail: String,
    items: Array,
    totalPrice: Number,
});
const Order = mongoose.model("Order", OrderSchema);

// Homepage - Show Available Food Items
app.get("/", async (req, res) => {
    const foods = await Food.find();
    res.render("index", { foods });
});

// Order Page - Handle Orders
app.post("/order", async (req, res) => {
    const { customerName, customerEmail, items, totalPrice } = req.body;
    const newOrder = new Order({ customerName, customerEmail, items: JSON.parse(items), totalPrice });
    await newOrder.save();
    res.redirect("/thank-you");
});

// Thank You Page
app.get("/thank-you", (req, res) => {
    res.render("thank-you");
});

// Start Server
app.listen(PORT, () => console.log(Server running on port ${PORT}));
