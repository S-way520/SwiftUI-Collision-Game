# SwiftUI-Collision-Game
A collision game where a small ball falls from the top and if it touches the slider below, it will have different results.
# The first part
https://github.com/S-way520/SwiftUI-Collision-Game/assets/95877651/b77c0191-f25a-445b-8e58-d3f2a6049a56
# The second part
Code file 1
```
import SwiftUI
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
Code file 2
```
import SwiftUI
struct ContentView: View {
    @State private var score = 0
    @State private var isColliding = false  
    @State private var RunGame = true
    
    @State var scaleMinValue = Double(0.1 * UIScreen.main.bounds.width)
    @State var scaleMaxValue = Double(UIScreen.main.bounds.width)
    
    @State private var circle1Position = CGPoint(x: UIScreen.main.bounds.width / 4, y: 30)
    @State private var circle2Position = CGPoint(x: UIScreen.main.bounds.width / 2, y: 30)
    @State private var circle3Position = CGPoint(x: UIScreen.main.bounds.width - 50, y: 30)

    @State private var TapPosition = CGPoint(x: 100, y: 750)

    func checkCollision() {
        let distance1 = sqrt(pow((circle1Position.x - TapPosition.x) / 2, 2) + pow(circle1Position.y - TapPosition.y, 2))
        let distance2 = sqrt(pow((circle2Position.x - TapPosition.x) / 2, 2) + pow(circle2Position.y - TapPosition.y, 2))
        let distance3 = sqrt(pow((circle3Position.x - TapPosition.x) / 2, 2) + pow(circle3Position.y - TapPosition.y, 2))
        if distance1 < 60 {
            if !isColliding {
                isColliding = true
                score += 100
            }
        } else if distance2 < 60 {
            if !isColliding {
                isColliding = true
                score += 10
            }
        } else if distance3 < 60 {
            if !isColliding {
                isColliding = true
                RunGame = false
               // score += 1
            }
        } else {
            isColliding = false
        }
    }
    var body: some View {
        ZStack {
            Text("Score: \(score)")
                .font(.title)
                .position(CGPoint(x: 200, y: 650))
            Circle()
                .frame(width: 30, height: 30)
                .foregroundColor(.red)
                .position(circle1Position)
                .onAppear {
                    Timer.scheduledTimer(withTimeInterval: 0.0025, repeats: true) { _ in
                        if RunGame && circle1Position.y <= UIScreen.main.bounds.height {
                            withAnimation {circle1Position.y = circle1Position.y + 1}
                        } else {
                            circle1Position.y = 0
                        }
                        checkCollision()
                    }
                }
            Circle()
                .frame(width: 30, height: 30)
                .foregroundColor(.green)
                .position(circle2Position)
                .onAppear {
                    Timer.scheduledTimer(withTimeInterval: 0.003, repeats: true) { _ in
                        if RunGame && circle2Position.y <= UIScreen.main.bounds.height {
                            withAnimation {circle2Position.y = circle2Position.y + 1}
                        } else {
                            circle2Position.y = 0
                        }
                    }
                }
            Circle()
                .frame(width: 30, height: 30)
                .foregroundColor(.blue)
                .position(circle3Position)
                .onAppear {
                    Timer.scheduledTimer(withTimeInterval: 0.002, repeats: true) { _ in
                        if RunGame && circle3Position.y <= UIScreen.main.bounds.height {
                            withAnimation {circle3Position.y = circle3Position.y + 1}
                        } else {
                            circle3Position.y = 0
                        }
                    }
                }
            Ellipse()
                .fill(.red)
                .frame(width: 240, height: 80)
                .position(CGPoint(x: scaleMinValue, y: UIScreen.main.bounds.height - 120))
                .gesture(DragGesture().onChanged({ value in
                    if RunGame && value.location.x >= -scaleMaxValue && value.location.x <= scaleMaxValue {
                        withAnimation{
                            scaleMinValue = value.location.x
                        }
                        TapPosition = CGPoint(x: scaleMinValue, y: UIScreen.main.bounds.height - 120)
                    }
                }))
                .sheet(isPresented: .constant(!RunGame), content: {
                    Text("Game Over")
                        .font(.largeTitle)
                })
        }
    }
}

```
