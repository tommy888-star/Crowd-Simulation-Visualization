let crowd = [];

function setup() {
  createCanvas(600, 400); // Create a space where our crowd will move
  for (let i = 0; i < 50; i++) { // Create 50 people (circles)
    let person = {
      x: random(width),  // Random starting position
      y: random(height),
      xSpeed: random(-1, 1), // Random movement speed
      ySpeed: random(-1, 1),
      size: 20  // Circle size (radius)
    };
    crowd.push(person);  // Add each person to our crowd
  }
}

function draw() {
  background(220);  // Gray background
  
  for (let i = 0; i < crowd.length; i++) {
    let person = crowd[i];
    
    // Move each person
    person.x += person.xSpeed;
    person.y += person.ySpeed;
    
    // Make sure they stay inside the screen
    if (person.x < 0 || person.x > width) person.xSpeed *= -1;
    if (person.y < 0 || person.y > height) person.ySpeed *= -1;

    // Check for collisions and adjust speed to avoid each other
    for (let j = 0; j < crowd.length; j++) {
      if (i != j) {  // Don't compare a person with themselves
        let other = crowd[j];
        let distance = dist(person.x, person.y, other.x, other.y); // Distance between two people
        
        if (distance < person.size) {  // If they are too close
          // Calculate the direction to move away
          let dx = person.x - other.x;
          let dy = person.y - other.y;
          
          // Normalize the direction and move away
          let angle = atan2(dy, dx);
          person.xSpeed += cos(angle) * 0.05;  // Move slightly away
          person.ySpeed += sin(angle) * 0.05;
        }
      }
    }

    // Draw each person as a circle
    ellipse(person.x, person.y, person.size, person.size);  // Circle with size 20
  }
}
