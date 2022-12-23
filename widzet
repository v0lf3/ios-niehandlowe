import WidgetKit
import SwiftUI

struct NonWorkingDaysWidget: Widget {
    let kind: String = "NonWorkingDaysWidget"

    var body: some WidgetConfiguration {
        StaticConfiguration(kind: kind, provider: Provider()) { entry in
            NonWorkingDaysWidgetEntryView(entry: entry)
        }
        .configurationDisplayName("Dni niehandlowe")
        .description("Wyświetla dni niehandlowe w bieżącym tygodniu")
    }
}

struct NonWorkingDaysWidgetEntryView: View {
    var entry: Provider.Entry

    var body: some View {
        Text(entry.nonWorkingDays)
            .font(.caption)
            .foregroundColor(.gray)
    }
}

struct Provider: TimelineProvider {
    typealias Entry = NonWorkingDaysEntry

    func snapshot(with context: Context, completion: @escaping (NonWorkingDaysEntry) -> ()) {
        let entry = NonWorkingDaysEntry(date: Date(), nonWorkingDays: getNonWorkingDays())
        completion(entry)
    }

    func timeline(with context: Context, completion: @escaping (Timeline<NonWorkingDaysEntry>) -> ()) {
        let currentDate = Date()
        let refreshDate = Calendar.current.date(byAdding: .minute, value: 5, to: currentDate)!

        let entry = NonWorkingDaysEntry(date: currentDate, nonWorkingDays: getNonWorkingDays())
        let timeline = Timeline(entries: [entry], policy: .after(refreshDate))

        completion(timeline)
    }

    func getNonWorkingDays() -> String {
        let currentWeekday = Calendar.current.component(.weekday, from: Date())
        let nonWorkingDays = [7, 1] // niedziele i poniedziałki są dniami niehandlowymi
        var nonWorkingDaysString = ""

        for i in 1...7 {
            if nonWorkingDays.contains(i) {
                nonWorkingDaysString += "X "
            } else {
                nonWorkingDaysString += "  "
            }
        }

        // przesuwamy string, aby aktualny dzień był na początku
        let index = nonWorkingDaysString.index(nonWorkingDaysString.startIndex, offsetBy: currentWeekday - 1)
        nonWorkingDaysString = String(nonWorkingDaysString.suffix(from: index)) + String(nonWorkingDaysString.prefix(upTo: index))

        return nonWorkingDaysString
    }
}

struct NonWorkingDaysEntry: TimelineEntry {
    let date: Date
    let nonWorkingDays: String
}
