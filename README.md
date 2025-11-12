# Voice Bot Hello World 🤖📞

A simple voice AI bot using Twilio and OpenAI that you can call and have a conversation with!

## What This Does

1. You call a Twilio phone number
2. An AI answers and greets you
3. You can have a natural conversation with it
4. The AI remembers context throughout the call

## Prerequisites

- Node.js installed (v14 or higher)
- A Twilio account with a phone number
- An OpenAI API key

## Setup Instructions

### 1. Install Dependencies

```bash
cd voice-bot
npm install
```

### 2. Configure Environment Variables

Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```

Then edit `.env` and add your credentials:

```
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxx
TWILIO_AUTH_TOKEN=your_auth_token
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxx
PORT=3000
```

**Where to find these:**
- Twilio credentials: https://console.twilio.com/
- OpenAI API key: https://platform.openai.com/api-keys

### 3. Run the Server

```bash
npm start
```

You should see: `Voice bot server running on port 3000`

### 4. Expose Your Server to the Internet

Since Twilio needs to reach your server, you need to expose it publicly. Use one of these options:

**Option A: ngrok (easiest for testing)**
```bash
# Install ngrok: https://ngrok.com/download
ngrok http 3000
```

This will give you a URL like: `https://abc123.ngrok.io`

**Option B: Deploy to a cloud service**
- Heroku
- Railway
- Render
- AWS/GCP/Azure

### 5. Configure Twilio Webhook

1. Go to your Twilio Console: https://console.twilio.com/
2. Navigate to Phone Numbers → Manage → Active numbers
3. Click on your phone number
4. Scroll down to "Voice Configuration"
5. Under "A CALL COMES IN", set:
   - **Webhook**: `https://your-domain.com/voice` (use your ngrok URL or deployment URL)
   - **HTTP Method**: POST
6. Under "Status Callback URL", set:
   - **Webhook**: `https://your-domain.com/voice/status`
   - **HTTP Method**: POST
7. Click "Save"

### 6. Test It!

Call your Twilio phone number! 🎉

The AI will:
1. Answer with a greeting
2. Listen to what you say
3. Respond using GPT-4
4. Continue the conversation

## How It Works

```
You Call → Twilio → Your Server → OpenAI → Response → Twilio → You Hear It
```

1. **Twilio receives the call** and sends a webhook to your `/voice` endpoint
2. **Your server responds** with TwiML (Twilio Markup Language) to greet and gather speech
3. **User speaks**, Twilio transcribes it, sends to `/voice/response`
4. **Your server sends the text** to OpenAI GPT-4
5. **GPT-4 responds**, your server converts to TwiML
6. **Twilio speaks the response** back to the caller
7. **Loop continues** until the call ends

## Customization Ideas

### Change the AI's personality
Edit the system message in `server.js`:
```javascript
{ 
  role: 'system', 
  content: 'You are a pirate AI assistant. Speak like a pirate!' 
}
```

### Change the voice
Twilio supports multiple voices. Options include:
- `Polly.Joanna` (female, US)
- `Polly.Matthew` (male, US)
- `Polly.Amy` (female, UK)
- `Polly.Brian` (male, UK)

See all voices: https://www.twilio.com/docs/voice/twiml/say/text-speech#amazon-polly

### Use a different AI model
Change the model in the OpenAI call:
```javascript
model: 'gpt-3.5-turbo', // Faster and cheaper
// or
model: 'gpt-4-turbo', // Smarter
```

## Troubleshooting

**"The application error: Cannot find module..."**
- Run `npm install` again

**"Call connects but I hear nothing"**
- Check your ngrok/server is running
- Verify the webhook URL in Twilio is correct
- Check server logs for errors

**"AI response is cut off"**
- Increase `max_tokens` in the OpenAI call
- The system prompt asks for brief responses (under 50 words)

**"High latency / slow responses"**
- Consider using `gpt-3.5-turbo` instead of `gpt-4`
- Deploy server closer to Twilio's servers (US East)

## Next Steps

Want to make it more advanced? Try:
- Add memory that persists across calls
- Integrate with your CRM or database
- Add call recording
- Implement call transfers to humans
- Add sentiment analysis
- Create a scheduling system
- Build multi-language support

## Cost Considerations

- **Twilio**: ~$0.01-0.02 per minute
- **OpenAI GPT-4**: ~$0.03 per 1K tokens (roughly $0.05-0.15 per conversation turn)
- **Total**: About $0.10-0.30 per minute of conversation

Use GPT-3.5-turbo to reduce costs by ~90%!

## Security Notes

- Never commit your `.env` file
- Rotate API keys regularly
- Add rate limiting for production
- Validate Twilio requests (add signature validation)
- Use HTTPS in production

## Resources

- [Twilio Voice Docs](https://www.twilio.com/docs/voice)
- [OpenAI API Docs](https://platform.openai.com/docs)
- [TwiML Reference](https://www.twilio.com/docs/voice/twiml)

---

Built with ❤️ using Twilio and OpenAI
