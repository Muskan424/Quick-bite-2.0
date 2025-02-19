// Food Delivery Backend with Server-Side Rendering (SSR)
// Using Node.js, Express, MongoDB, and EJS

const express = require("express");
const mongoose = require("mongoose");
const path = require("path");
const bodyParser = require("body-parser");

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(bodyParser.urlencoded({ extended: true }));
app.use(express.static("public")); // For static files (CSS, images)
app.set("view engine", "ejs");

// Connect to MongoDB
mongoose.connect("mongodb://localhost:27017/foodDelivery", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
}).then(() => console.log("MongoDB connected"))
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
    items: Array,
    totalPrice: Number,
    customerName: String,
    customerEmail: String,
});
const Order = mongoose.model("Order", OrderSchema);

// Routes
app.get("/", async (req, res) => {
    const foods = await Food.find();
    res.render("index", { foods });
});

app.post("/order", async (req, res) => {
    const { customerName, customerEmail, items, totalPrice } = req.body;
    const newOrder = new Order({ customerName, customerEmail, items: JSON.parse(items), totalPrice });
    await newOrder.save();
    res.redirect("/thank-you");
});

app.get("/thank-you", (req, res) => {
    res.render("thank-you");
});

// Start Server
app.listen(PORT, () => console.log(Server running on port ${PORT}));
