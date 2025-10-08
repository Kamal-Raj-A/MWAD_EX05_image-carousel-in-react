# MWAD_EX05_image-carousel-in-react
## Date: 08-10-2025

## AIM
To create a Image Carousel using React 

## ALGORITHM
### STEP 1 Initial Setup:
Input: A list of images to display in the carousel.

Output: A component displaying the images with navigation controls (e.g., next/previous buttons).

### Step 2 State Management:
Use a state variable (currentIndex) to track the index of the current image displayed.

The carousel starts with the first image, so initialize currentIndex to 0.

### Step 3 Navigation Controls:
Next Image: When the "Next" button is clicked, increment currentIndex.

If currentIndex is at the end of the image list (last image), loop back to the first image using modulo:
currentIndex = (currentIndex + 1) % images.length;

Previous Image: When the "Previous" button is clicked, decrement currentIndex.

If currentIndex is at the beginning (first image), loop back to the last image:
currentIndex = (currentIndex - 1 + images.length) % images.length;

### Step 4 Displaying the Image:
The currentIndex determines which image is displayed.

Using the currentIndex, display the corresponding image from the images list.

### Step 5 Auto-Rotation:
Set an interval to automatically change the image after a set amount of time (e.g., 3 seconds).

Use setInterval to call the nextImage() function at regular intervals.

Clean up the interval when the component unmounts using clearInterval to prevent memory leaks.

## PROGRAM
### ImageCarousel.js

```
import React, { useState, useEffect } from "react";
import "./ImageCarousel.css";

const ImageCarousel = () => {
  // ✅ Step 1: Image list with captions
  const images = [
    { url: "https://picsum.photos/id/1018/900/450", caption: "Mountain Serenity" },
    { url: "https://picsum.photos/id/1015/900/450", caption: "Golden Sunset" },
    { url: "https://picsum.photos/id/1016/900/450", caption: "Mirror Lake Reflections" },
    { url: "https://picsum.photos/id/1021/900/450", caption: "Road to Adventure" },
  ];

  // ✅ Step 2: State for current image & hover
  const [currentIndex, setCurrentIndex] = useState(0);
  const [isHovered, setIsHovered] = useState(false);

  // ✅ Step 3: Navigation controls
  const nextImage = () => {
    setCurrentIndex((prev) => (prev + 1) % images.length);
  };

  const prevImage = () => {
    setCurrentIndex((prev) => (prev - 1 + images.length) % images.length);
  };

  // ✅ Step 4: Auto-Rotation (Pause on hover)
  useEffect(() => {
    if (isHovered) return; // pause when hovered
    const interval = setInterval(nextImage, 3000);
    return () => clearInterval(interval);
  }, [isHovered]);

  return (
    <div
      className="carousel-container"
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      {/* Navigation Buttons */}
      <button className="prev-btn" onClick={prevImage}>❮</button>

      {/* Image + Caption */}
      <div className="image-wrapper">
        <img
          src={images[currentIndex].url}
          alt={`Slide ${currentIndex}`}
          className="carousel-image fade-in"
        />
        <p className="carousel-caption">{images[currentIndex].caption}</p>
      </div>

      <button className="next-btn" onClick={nextImage}>❯</button>

      {/* Dots Indicators */}
      <div className="indicators">
        {images.map((_, index) => (
          <span
            key={index}
            className={`dot ${index === currentIndex ? "active" : ""}`}
            onClick={() => setCurrentIndex(index)}
          ></span>
        ))}
      </div>
    </div>
  );
};

export default ImageCarousel;
```

### ImageCarousel.css

```
/* ===== Overall Layout ===== */
.carousel-container {
  position: relative;
  width: 80%;
  max-width: 900px;
  height: 450px;
  margin: 60px auto;
  overflow: hidden;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
  background: linear-gradient(145deg, #111, #222);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.3s ease;
}

.carousel-container:hover {
  transform: scale(1.02);
}

/* ===== Image Style ===== */
.carousel-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 20px;
  transition: opacity 1s ease-in-out, transform 1s ease;
}

/* ===== Buttons ===== */
.prev-btn, .next-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255, 255, 255, 0.2);
  border: 2px solid rgba(255, 255, 255, 0.5);
  color: #fff;
  font-size: 2.5rem;
  padding: 10px 16px;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(6px);
  z-index: 10;
}

.prev-btn:hover, .next-btn:hover {
  background: rgba(255, 255, 255, 0.8);
  color: #000;
  box-shadow: 0 0 20px #fff;
}

.prev-btn {
  left: 20px;
}

.next-btn {
  right: 20px;
}

/* ===== Indicators (Dots) ===== */
.indicators {
  text-align: center;
  position: absolute;
  bottom: 15px;
  width: 100%;
}

.dot {
  display: inline-block;
  height: 12px;
  width: 12px;
  margin: 0 5px;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.dot:hover {
  background-color: #fff;
  transform: scale(1.3);
}

.active {
  background-color: #fff;
  width: 15px;
  height: 15px;
}

/* ===== Caption (optional) ===== */
.carousel-caption {
  position: absolute;
  bottom: 60px;
  width: 100%;
  text-align: center;
  color: #fff;
  font-size: 1.4rem;
  text-shadow: 2px 2px 10px rgba(0, 0, 0, 0.7);
  font-family: 'Poppins', sans-serif;
  animation: fadeInUp 1s ease;
}

/* ===== Animations ===== */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* ===== Responsive Design ===== */
@media (max-width: 768px) {
  .carousel-container {
    height: 300px;
    width: 95%;
  }

  .prev-btn, .next-btn {
    font-size: 1.8rem;
    padding: 8px 12px;
  }

  .carousel-caption {
    font-size: 1rem;
    bottom: 40px;
  }
}
```

### App.js

```
import React from "react";
import ImageCarousel from "./ImageCarousel";

function App() {
  return (
    <div>
      <h1 style={{ textAlign: "center" }}>React Image Carousel</h1>
      <ImageCarousel />
    </div>
  );
}
export default App;
```

## OUTPUT
<img width="1918" height="1002" alt="image" src="https://github.com/user-attachments/assets/e254974f-8c49-4366-8fee-1cf151a15fe5" />
<img width="1919" height="1013" alt="image" src="https://github.com/user-attachments/assets/02d72510-5156-4230-9c70-a2b8012720df" />


## RESULT
The program for creating Image Carousel using React is executed successfully.
