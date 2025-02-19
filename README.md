
// Backend for Food Delivery App
// Using Node.js, Express, and MongoDB

const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(express.json());
app.use(cors());

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

// Routes
app.get("/api/foods", async (req, res) => {
    const foods = await Food.find();
    res.json(foods);
});

app.post("/api/foods", async (req, res) => {
    const { name, price, image } = req.body;
    const newFood = new Food({ name, price, image });
    await newFood.save();
    res.json(newFood);
});

// Order Schema
const OrderSchema = new mongoose.Schema({
    items: Array,
    totalPrice: Number,
    customerName: String,
    customerEmail: String,
});

const Order = mongoose.model("Order", OrderSchema);

app.post("/api/orders", async (req, res) => {
    const { items, totalPrice, customerName, customerEmail } = req.body;
    const newOrder = new Order({ items, totalPrice, customerName, customerEmail });
    await newOrder.save();
    res.json({ message: "Order placed successfully" });
});

// Start Server
app.listen(PORT, () => console.log(Server running on port ${PORT}));
