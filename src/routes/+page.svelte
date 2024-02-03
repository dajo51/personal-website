<script>
  import { onMount } from "svelte";
  onMount(() => {
    // Start with initializing cursor elements and mouse position
    const cursorInner = document.querySelector(".cursor--small");
    const cursorOuter = document.querySelector(".cursor--large");
    const cursorTextContainerEl = document.querySelector(".cursor--text");
    const cursorTextEl = cursorTextContainerEl.querySelector(".text");

    let mouse = { x: -100, y: -100 }; // Initial mouse position
    let isHovered = false; // State to track if hovering over an item

    // Event listeners for mouse movement and hover items
    document.body.addEventListener("mousemove", updateCursorPosition);
    const hoverItems = document.querySelectorAll(".cursor-hover-item");
    hoverItems.forEach((item) => {
      item.addEventListener("mouseenter", handlePointerEnter);
      item.addEventListener("mouseleave", handlePointerLeave);
    });

    // Function to update cursor position based on mouse movement
    function updateCursorPosition(e) {
      mouse.x = e.clientX;
      mouse.y = e.clientY;
    }

    // Function to animate the cursor
    function updateCursor() {
      cursorInner.style.transform = `translate3d(${mouse.x}px, ${mouse.y}px, 0)`;
      cursorOuter.style.transform = `translate3d(${mouse.x}px, ${mouse.y}px, 0)`;

      if (!isHovered) {
        cursorTextContainerEl.style.opacity = 0;
      }

      requestAnimationFrame(updateCursor);
    }
    updateCursor();

    // Function to handle mouse enter on hover items
    function handlePointerEnter(e) {
      isHovered = true;
      const text = e.target.getAttribute("data-cursor-text");
      const repeat = parseInt(
        e.target.getAttribute("data-cursor-text-repeat"),
        10
      );
      cursorTextEl.textContent = Array(repeat).fill(text).join(" ");
      cursorTextContainerEl.style.opacity = 1;
    }

    // Function to handle mouse leave on hover items
    function handlePointerLeave() {
      isHovered = false;
    }
  });
</script>

<main class="flex justify-center items-center h-screen bg-gray-800">
  <div class="text-center">
    <div class="mb-4">
      <!-- Links -->
      <a
        class="link cursor-hover-item"
        href="#"
        data-cursor-text="GO HERE!"
        data-cursor-text-repeat="4">Link</a
      >
      <a
        class="link cursor-hover-item"
        href="#"
        data-cursor-text="LEARN MORE!"
        data-cursor-text-repeat="3">Read More</a
      >
      <a
        class="link cursor-hover-item"
        href="#"
        data-cursor-text="FOLLOW ME!"
        data-cursor-text-repeat="4">Follow Me</a
      >
    </div>
    <h1 class="text-4xl text-white">Big Headline</h1>
  </div>
</main>

<!-- Cursor HTML -->
<div class="cursor">
  <div class="cursor--small"></div>
  <div class="cursor--large"></div>
  <div class="cursor--text">
    <div class="text"></div>
  </div>
</div>

<style>
  /* Custom Cursor Styles */
  .cursor {
    position: fixed;
    pointer-events: none;
    z-index: 1000;
  }

  .cursor--small,
  .cursor--large,
  .cursor--text {
    position: fixed;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    mix-blend-mode: difference;
    pointer-events: none;
    user-select: none;
  }

  .cursor--small {
    width: 20px;
    height: 20px;
    background-color: #fff;
  }

  .cursor--large {
    width: 60px;
    height: 60px;
    border: 2px solid #fff;
    opacity: 0;
    transition:
      opacity 0.3s ease,
      transform 0.3s ease;
  }

  .cursor--text {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 4px;
    background-color: rgba(0, 0, 0, 0.6);
    color: #fff;
    border: none;
    opacity: 0;
    transition:
      opacity 0.3s ease,
      transform 0.3s ease;
  }

  /* Link Styles */
  .link {
    color: rgba(255, 255, 255, 0.5);
    text-decoration: none;
    margin: 0 1rem;
    transition: color 300ms ease;
  }

  .link:hover {
    color: #fff;
  }

  /* Tailwind Utility Classes can be used directly in the HTML part */
</style>
