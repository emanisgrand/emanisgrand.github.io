# emanisgrand.github.io

---

### In the Loop: Spinning from Building Blocks to Google Blocks

In 2017, I joined the development team of Google Blocks, an award-winning VR application designed to democratize 3D modeling, making it accessible to all. As a newcomer amidst the brilliant minds shaping the future of virtual reality, the specter of imposter syndrome loomed large. Yet, it was a time brimming with exploratory fervor, and the ethos of the team I was part of was nothing short of empowering.

Encouraged to push boundaries and explore the uncharted, I delved into the intricacies of gyroscopes and accelerometers. These weren't just components of the smartphones we use every day; they were the keys to unlocking new dimensions in virtual reality. This exploration wasn't a diversion but a vital part of our collective journey into the unknown. The supportive environment at Google Blocks made it clear that taking the time to understand and experiment with the hardware we were using wasn't just permissible—it was indispensable.

As I navigated through my initial doubts and the complexities of the technology, the venture into the depths of gyroscopes and accelerometers became more than just a task; it became a gateway to innovation in VR, laying the groundwork for advancements I could have hardly imagined at the time.

---
### The Basics of Gyroscopes and Accelerometers

At the heart of our smartphones, hidden beneath sleek screens and glossy exteriors, lie gyroscopes and accelerometers. These sensors might not capture our attention like high-resolution cameras or vibrant displays, but they are fundamental to the way our devices interact with the world.

#### Gyroscopes:
Imagine standing in the center of a spinning merry-go-round, holding your position steady as the world whirls around you. This is the essence of a gyroscope. It measures orientation and angular velocity, helping devices understand their rotation in space. In simpler terms, a gyroscope in your phone helps it know whether it's being tilted, turned, or spun.

#### Accelerometers:
Now, think of an elevator briskly ascending, pressing you gently into the floor. An accelerometer measures this type of acceleration - not just the speed, but the change in velocity. Whether you're walking, jumping, or just holding your phone, the accelerometer tracks these movements in any direction.

These sensors, often working in tandem, allow our phones to perform tasks we take for granted, like switching from portrait to landscape view, tracking our steps, or stabilizing our photos. But their utility goes beyond just these everyday conveniences; they are also foundational elements in the development of immersive VR experiences.

#### From Phones to VR:
In a VR headset, gyroscopes and accelerometers serve a similar but amplified purpose. They track the user's head movements with precision, translating real-world movements into virtual ones. This seamless integration of physical and digital movement is what makes VR truly immersive.

Absolutely, let's integrate more technical elements into this section, focusing on quaternion mathematics and its application in VR through a specific formula and accompanying code example.

---

### Delving into the Technicalities: Quaternions in VR

As we venture deeper into the technical backbone of VR, we encounter quaternions, a sophisticated mathematical tool that elegantly handles the complexities of 3D rotation and orientation. Quaternions extend beyond traditional 3D vector space, offering a more robust and less cumbersome solution than Euler angles, which can suffer from gimbal lock.

#### Quaternions Explained:
A quaternion is a four-part number system that can represent orientations and rotations in three-dimensional space more efficiently than other methods. A quaternion is typically represented as \( Q = a + bi + cj + dk \), where \( a, b, c, \) and \( d \) are real numbers, and \( i, j, \), and \( k \) are the fundamental quaternion units.

For VR applications, quaternions are pivotal in calculating the orientation of the headset. They allow for smooth, continuous rotation without the pitfalls of gimbal lock, ensuring a more immersive and comfortable user experience.

#### Quaternion Rotation Formula:
One fundamental operation with quaternions in VR is rotating a vector by a quaternion. The rotation of a vector \( \vec{v} \) by a quaternion \( Q \) can be represented as \( \vec{v}_{rot} = Q \vec{v} Q^{-1} \), where \( Q^{-1} \) is the inverse of \( Q \). This formula is crucial for determining how objects or views rotate in response to headset movements.

#### Code Example: Quaternion-Based Rotation
I'll make a code snippet that demonstrates quaternion-based rotation, which could be part of a VR system:
Certainly! Let's convert the Python code example to C, focusing on the quaternion-based rotation operation:

```c
#include <stdio.h>
#include <math.h>

// Quaternion structure
typedef struct {
    double w, x, y, z;
} Quaternion;

// Function to normalize a quaternion
Quaternion normalizeQuaternion(Quaternion q) {
    double norm = sqrt(q.w * q.w + q.x * q.x + q.y * q.y + q.z * q.z);
    q.w /= norm;
    q.x /= norm;
    q.y /= norm;
    q.z /= norm;
    return q;
}

// Function to invert a quaternion
Quaternion inverseQuaternion(Quaternion q) {
    q.x = -q.x;
    q.y = -q.y;
    q.z = -q.z;
    return q;  // Assuming quaternion is already normalized
}

// Quaternion multiplication
Quaternion multiplyQuaternions(Quaternion q1, Quaternion q2) {
    Quaternion result;
    result.w = q1.w*q2.w - q1.x*q2.x - q1.y*q2.y - q1.z*q2.z;
    result.x = q1.w*q2.x + q1.x*q2.w + q1.y*q2.z - q1.z*q2.y;
    result.y = q1.w*q2.y - q1.x*q2.z + q1.y*q2.w + q1.z*q2.x;
    result.z = q1.w*q2.z + q1.x*q2.y - q1.y*q2.x + q1.z*q2.w;
    return result;
}

// Function to rotate a vector using a quaternion
void rotateVectorByQuaternion(double vector[3], Quaternion q, double rotatedVector[3]) {
    // Create a quaternion from the vector
    Quaternion vectorQ = {0, vector[0], vector[1], vector[2]};
    
    // Normalize the quaternion to ensure it represents a valid rotation
    q = normalizeQuaternion(q);

    // Calculate the inverse of the quaternion
    Quaternion qInverse = inverseQuaternion(q);

    // Rotate the vector: v_rot = q * v * q^(-1)
    Quaternion temp = multiplyQuaternions(q, vectorQ);
    Quaternion rotatedVectorQ = multiplyQuaternions(temp, qInverse);

    // Extract the rotated vector
    rotatedVector[0] = rotatedVectorQ.x;
    rotatedVector[1] = rotatedVectorQ.y;
    rotatedVector[2] = rotatedVectorQ.z;
}

int main() {
    // Example vector and quaternion
    double vector[3] = {1, 0, 0};  // Arbitrary vector
    Quaternion quaternion = {1, 0, 1, 0};  // Arbitrary quaternion
    double rotatedVector[3];

    // Rotate the vector using the quaternion
    rotateVectorByQuaternion(vector, quaternion, rotatedVector);

    printf("Rotated Vector: [%f, %f, %f]\n", rotatedVector[0], rotatedVector[1], rotatedVector[2]);

    return 0;
}
```

This C code snippet defines a `Quaternion` structure, provides functions for normalizing and inverting quaternions, and implements quaternion multiplication to apply the rotation. The `rotateVectorByQuaternion` function demonstrates how to use these operations to rotate a 3D vector using quaternion rotation, which is essential for calculating the orientation changes in a VR headset.

---

### Reflecting on the Journey

As we reach the end of this technical voyage, it's worth pausing to reflect on the serendipitous nature of innovation. My journey from prototyping phone features at Google to unwittingly laying the groundwork for modern VR technology is a testament to the unpredictable paths that technological advancements often take.

#### The Unseen Impact of Exploration:
My work with gyroscopes and accelerometers was driven by curiosity and the pursuit of enhancing user experience on mobile devices. Little did I know, the principles and algorithms we were experimenting with would become cornerstones in the rapidly evolving field of VR. This highlights the intrinsic value of exploration and experimentation, where the impact of today's innovations can extend far beyond their original scope.

#### Bridging Worlds:
The transition from using sensors in smartphones to facilitating immersive experiences in VR showcases the remarkable versatility of these technologies. It also underscores the importance of interdisciplinary knowledge and the blending of ideas from seemingly unrelated fields. The leap from a handheld device to an immersive virtual environment is both a technological marvel and a creative leap, made possible by viewing familiar tools through a new lens.

#### The Future of VR and Beyond:
As we stand on the brink of what VR can become, it's exciting to ponder the future possibilities. The foundational work in sensor technology and inverse camera tracking is just the beginning. The next frontier involves enhancing realism, reducing latency, and creating more intuitive interactions within virtual spaces. The journey from simple sensor applications to complex VR systems illustrates the iterative nature of innovation, where each advancement builds upon the last, pushing the boundaries of what's possible.

---

---
### [LinkedIn](https://www.linkedin.com/in/emanuel-martinez-80118760/)
### [Google Blocks](https://sites.google.com/view/em4ngifs/home)
### [Licenses & Certifications](https://sites.google.com/view/em4ngifs/38?authuser=0)

