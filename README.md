import SwiftUI
struct ResultView: View {
    private let lineChartWidth = UIScreen.main.bounds.width - 40
    private let gdWidth = UIScreen.main.bounds.width
    @State var isAnimationg = false
    var body: some View {
        VStack {
            FitnessTopTitleView()
                .opacity(isAnimationg ? 1 : 0)
                .animation(Animation.spring().delay(0))
            ListTimeView()
                .opacity(isAnimationg ? 1 : 0)
                .animation(Animation.spring().delay(0.2))
            ZStack{
                Rectangle()
                    .fill(bgColors)
                    .edgesIgnoringSafeArea(.all)
                VStack{
                    Spacer()
                    Text("数据图表")
                        .multilineTextAlignment(.leading)
                        .font(.system(size: 24))
                        .foregroundColor(.white)
                        .frame(width:lineChartWidth)
                        .opacity(isAnimationg ? 1 : 0)
                        .animation(Animation.spring().delay(0.4))
                    Spacer()
                    ChartView()
                        .opacity(isAnimationg ? 1 : 0)
                        .animation(Animation.spring().delay(0.6))
                    Spacer()
                    Button(action: {}) {
                        Text("结果")
                            .frame(width: gdWidth - 80, height: 50)
                            .background(LinearGradient(gradient: Gradient.init(colors: [Color.init(red: 117/255, green: 70/255, blue: 234/255),Color.init(red: 214/255, green: 61/255, blue: 178/255)]),startPoint: .leading,endPoint: .trailing))
                            .cornerRadius(25)
                            .font(.system(size: 20))
                            .foregroundColor(.white)
                            .shadow(radius: 10)
                    }
                    .opacity(isAnimationg ? 1 : 0)
                    .animation(Animation.spring().delay(0.8))
                    Spacer()
                }
            }
        }
        .navigationBarTitle("")
        .navigationBarHidden(true)
        .onAppear(){
            self.isAnimationg.toggle()
        }
    }
}
struct FitnessTopTitleView: View {
    @Environment(\.presentationMode) var presentationMode:Binding<PresentationMode>
    var body: some View {
        HStack {
            Button(action:{
                self.presentationMode.wrappedValue.dismiss()
            }){
                Image(systemName: "chevron.left")
            }
            Spacer()
            Text("健身数据")
                .font(.system(size: 20))
                .bold()
            Spacer()
        }
        .padding(.leading,20)
        .padding(.trailing,20)
        .foregroundColor(.black)
    }
}
struct Splitter: View {
    var body: some View {
        Rectangle()
            .fill(Color.gray)
            .frame(height:1)
    }
}
struct ListTimeView: View {
    private let name = ["平均时间","最快时间","最慢时间","总距离"]
    private let time = ["6:52","7:12","8:21","9公里"]
    var body: some View {
        VStack{
            HStack{
                Text("每日数据")
                    .font(.system(size: 20))
                Button(action: {}) {
                    ZStack{
                        RoundedRectangle(cornerRadius: 20)
                            .fill(bgColors)
                            .frame(width: 80, height: 36)
                        Text("更多")
                            .foregroundColor(.white)
                            .font(.system(size: 14))
                    }
                    .frame(width: columuWidths/2, height: 36)
                }
            }
            ForEach(0..<4){i in
                Splitter()
                HStack{
                    Text(self.name[i])
                        .foregroundColor(self.name[i] == "总距离" ? .purple : .black)
                        .font(.system(size: 14))
                        .frame(width: columuWidths, height: 36)
                    Spacer()
                    Text(self.time[i])
                        .font(.system(size: 14))
                        .frame(width: columuWidths/2, height: 36)
                }
            }
        }
    }
}
struct ChartView: View {
    private let barHeights = [160,200,170,150,130,100,140]
    private let num1 = [0.9,1.0,1.1,1.2,1.3,1.4,1.5,1.6]
    @State private var isAnimationg = false
    var body: some View {
        ZStack(alignment:.leading){
            HStack(alignment:.bottom){
                ForEach (0..<7){ i in
                    Spacer()
                    RoundedRectangle(cornerRadius: 10)
                        .fill(Color.init(red: 214/255, green: 61/255, blue: 178/255))
                        .frame(width:20 , height:self.isAnimationg ? CGFloat(self.barHeights[i]) : 0)
                        .animation(Animation.interpolatingSpring(stiffness: 100, damping: 10).delay(self.num1[i]))
                    Spacer()
                }
            }
            .onAppear{
                self.isAnimationg.toggle()
            }
            Path{ pat in
                pat.move(to: CGPoint(x: 0, y: 100))
                pat.addQuadCurve(to: CGPoint(x: 130, y: 30), control: CGPoint(x: 60, y: -10))
                pat.addQuadCurve(to: CGPoint(x: 230, y: 90), control: CGPoint(x: 180, y: 60))
                pat.addQuadCurve(to: CGPoint(x: lineChartWidth, y: 60), control: CGPoint(x: 300, y: 130))
                pat.addLine(to:CGPoint(x: lineChartWidth, y: 200))
                pat.addLine(to:CGPoint(x: 0, y: 200))
                pat.addLine(to:CGPoint(x: 0, y: 100))
            }
            .fill(Color.white.opacity(0.1))
            .scaleEffect(x: 1, y: isAnimationg ? 1 : 0, anchor: .bottom)
            .animation(Animation.interpolatingSpring(stiffness: 100, damping: 10).delay(0.8))
            Path{ path in
                path.move(to: CGPoint(x: 0, y: 100))
                path.addQuadCurve(to: CGPoint(x: 130, y: 30), control: CGPoint(x: 60, y: -10))
                path.addQuadCurve(to: CGPoint(x: 230, y: 90), control: CGPoint(x: 180, y: 60))
                path.addQuadCurve(to: CGPoint(x: lineChartWidth, y: 60), control: CGPoint(x: 300, y: 130))
            }
            .stroke(lineWidth: 2)
            .fill(Color.white)
            .scaleEffect(x: 1, y: isAnimationg ? 1 : 0, anchor: .bottom)
            .animation(Animation.interpolatingSpring(stiffness: 100, damping: 10).delay(0.8))
            ZStack{
                Circle()
                    .stroke(lineWidth: 2)
                    .frame(width: 20, height: 20)
                Circle()
                    .frame(width: 6, height: 6)
            }
            .foregroundColor(.white)
            .offset(x: 70, y:isAnimationg ? -80 : 0)
            .animation(Animation.interpolatingSpring(stiffness: 100, damping: 10).delay(0.8))
        }
        .frame(width: lineChartWidth, height: 200)
    }
}
struct ResultView_Previews: PreviewProvider {
    static var previews: some View {
        ResultView()
    }
}
