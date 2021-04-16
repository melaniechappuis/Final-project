     #pragma once

     #include "ofMain.h"
     
     #include "ofxMidi.h"
     
     #define MAX_NUM_OF_NOTES 1000
     
     typedef struct
    {
    ofPoint pos;
    int time_counter;
    int note_num;
    int note_vel;
    
    } NoteData;
    
    class ofApp :   public ofBaseApp,
                public ofxMidiListener
     {
	
    public:
    
    void setup();
	void update();
	void draw();
	void exit();
    void keyPressed(int key);
    
    void newMidiMessage(ofxMidiMessage& eventArgs);
    
    private:
    
    NoteData noteData[MAX_NUM_OF_NOTES];
    
    int backgroundColour;
    
    ofxMidiIn midiIn;
    
     bool showingInstructions;
    };

