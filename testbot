package main

import (
	"fmt"
	"strings"

	"github.com/nlopes/slack"
)

func main() {
	token := "xoxb-360783986946-363437162342-KaVIO4xNd9E3yETpXu2XBp6U"
	api := slack.New(token)
	rtm := api.NewRTM()
	go rtm.ManageConnection()

	for {
		select {
		case msg := <-rtm.IncomingEvents:
			fmt.Println(`Event Received: `)
			switch ev := msg.Data.(type) {
			case *slack.ConnectedEvent:
				fmt.Println(`Connection Counter: `, ev.ConnectionCount)

			case *slack.MessageEvent:
				fmt.Printf("Message: %v\n", ev)
				info := rtm.GetInfo()
				prefix := fmt.Sprintf("<@%s> ", info.User.ID)

				if ev.User != info.User.ID && strings.HasPrefix(ev.Text, prefix) {
					respond(rtm, ev, prefix)
				}
			case *slack.RTMError:
				fmt.Printf("Error: %s\n", ev.Error())

			case *slack.InvalidAuthEvent:
				fmt.Printf("Invalid credentials")
				break

			default:

			}

		}
	}
}

func respond(rtm *slack.RTM, msg *slack.MessageEvent, prefix string) {
	var response string
	text := msg.Text
	text = strings.TrimPrefix(text, prefix)
	text = strings.TrimSpace(text)
	text = strings.ToLower(text)

	acceptedGreetings := map[string]bool{
		"whats up?": true,
		"hey":       true,
		"yo":        true,
	}

	acceptedHpwAreYou := map[string]bool{
		"how its going?": true,
		"how are ya?":    true,
		"feeling okay?":  true,
	}

	if acceptedGreetings[text] {
		response = "Whats up buddy?"
		rtm.SendMessage(rtm.NewOutgoingMessage(response, msg.Channel))
	} else if acceptedHpwAreYou[text] {
		response = "good. How are you?"
		rtm.SendMessage(rtm.NewOutgoingMessage(response, msg.Channel))
	}
}
