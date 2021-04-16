    #include "ofApp.h"

    void ofApp::setup()
    {
    ofSetVerticalSync(true);
    ofSetLogLevel(OF_LOG_VERBOSE);
    ofSetFrameRate(60);
    
    midiIn.openPort(0);
    
    midiIn.addListener(this);

    for (int i = 0; i < MAX_NUM_OF_NOTES; i++)
    {
        noteData[i].time_counter = 0;
    }
        ofBackground(255,0,0);
        
     showingInstructions = true;
    }  
    
    void ofApp::newMidiMessage (ofxMidiMessage& msg)
    {
    
    if (msg.status == MIDI_NOTE_ON && msg.velocity != 0)
    {
    
    for (int i = 0; i < MAX_NUM_OF_NOTES; i++)
        {
        
    if (noteData[i].time_counter == 0)
        {
        
        noteData[i].time_counter = 255;
        
        noteData[i].pos.set (ofRandom (ofGetWidth()), ofRandom (ofGetHeight()));
        
        noteData[i].note_num = msg.pitch;
                noteData[i].note_vel = msg.velocity;
                
                 break;
                 
                      }    
                   }       
                }
                
                else if (msg.status == MIDI_CONTROL_CHANGE && msg.control == 1)
        {
    
       backgroundColour = msg.value;
        
        }
    }
    
    void ofApp::update()
    {    
    }
    
    void ofApp::draw()
    {
    
    ofColor colorOne (255, 0, 0);
    ofColor colorTwo (0, 0, 255);
    
    ofBackgroundGradient(colorOne, colorTwo, OF_GRADIENT_CIRCULAR);
    
    for (int i = 0; i < MAX_NUM_OF_NOTES; i++)
    {
   
    if (noteData[i].time_counter > 0)
        {
    
    int alpha = noteData[i].time_counter;
    
    int colour = noteData[i].note_num * 2;
    
    ofSetColor(255-colour, colour, colour, alpha);
    
    ofCircle(noteData[i].pos, noteData[i].note_vel / 2);
    
    noteData[i].time_counter--;
    
        }
     }
     
     if (showingInstructions)
    {
    
    ofSetColor(255);
    
    ofDrawBitmapString("MIDI inputs available:", 10, 24);
    
     vector<string> midi_port_strings = midiIn.getInPortList();
     
     int last_line_pos = 44;
     
     for (int i = 0; i < midi_port_strings.size(); i++)
        {
            ofDrawBitmapString(ofToString(i) + ": " + midi_port_strings[i], 10, 94 + (10 * i));
            
            last_line_pos = 44 + (20 * i);
        }
        
        ofDrawBitmapString("Currently connected to MIDI input:", 10, last_line_pos + 40);
        
        if (midiIn.getPort() != -1)
            ofDrawBitmapString(midiIn.getName(), 10, last_line_pos + 60);
            
            else
            ofDrawBitmapString("none/invalid", 10, last_line_pos + 60);
        
        ofDrawBitmapString("Press 0-9 on the keyboard to set the connected port number.", 10, last_line_pos + 100);
        ofDrawBitmapString("Press lowercase 's' on the keyboard to show/hide this text.", 10, last_line_pos + 120);
        
           }
         }
         
         void ofApp::keyPressed(int key)
    {
    
    if (key == 's')
    {
    
    showingInstructions = !showingInstructions;
    }
    
    else if (key >= '0' && key <= '9')
    {
    
    int port_num = key - 48;
    
    midiIn.closePort();
    
    midiIn.openPort(port_num);
    }
    }

    void ofApp::exit()
    {
    
    midiIn.closePort();
    
     midiIn.removeListener(this);
    }
