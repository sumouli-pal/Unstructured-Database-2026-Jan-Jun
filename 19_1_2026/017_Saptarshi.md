import express from "express";
import { createClient } from "redis";

const app = express();
const PORT = 3000;

// Redis client
const redisClient = createClient();
await redisClient.connect();

// Rate limit config
const RATE_LIMIT = 5;      // max requests
const WINDOW = 60;         // time window in seconds

// Rate limiter middleware
const rateLimiter = async (req, res, next) => {
  const ip = req.ip;
  const key = `rate:${ip}`;

  try {
    const count = await redisClient.incr(key);

    if (count === 1) {
      await redisClient.expire(key, WINDOW);
    }

    if (count > RATE_LIMIT) {
      return res.status(429).json({
        success: false,
        message: "Too many requests. Try again later."
      });
    }

    next();
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: "Redis error" });
  }
};

// Apply rate limiter
app.use(rateLimiter);
console.log("Node.js file is running successfully ✅");

app.get("/", (req, res) => {
  res.send("Hello! Request allowed ✅");
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
