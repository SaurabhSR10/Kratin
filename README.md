# Kratin
import React, { useState, useEffect } from "react";

const MedicationReminder = () => {
  const [medications, setMedications] = useState([]);
  const [reminders, setReminders] = useState([]);
  const [adherenceData, setAdherenceData] = useState([]);

  useEffect(() => {
    // Fetch medications from the backend server
    fetch("/medications")
      .then((response) => response.json())
      .then((data) => setMedications(data));
  }, []);

  const addMedication = (medication) => {
    setMedications([...medications, medication]);
  };

  const removeMedication = (medication) => {
    const updatedMedications = medications.filter(
      (m) => m.id !== medication.id
    );
    setMedications(updatedMedications);
  };

  const addReminder = (medication, time) => {
    setReminders([...reminders, { medication, time }]);
  };

  const removeReminder = (reminder) => {
    const updatedReminders = reminders.filter(
      (r) => r.medication.id !== reminder.medication.id && r.time !== reminder.time
    );
    setReminders(updatedReminders);
  };

  const trackAdherence = (medication) => {
    const updatedAdherenceData = adherenceData.map((m) => {
      if (m.medication.id === medication.id) {
        m.taken = true;
      }
      return m;
    });
    setAdherenceData(updatedAdherenceData);
  };

  const sendAlert = (medication) => {
    // TODO: Send an alert to the user's family or caregivers
  };

  return (
    <div>
      <h1>Medication Reminder</h1>
      <ul>
        {medications.map((medication) => (
          <li key={medication.id}>
            {medication.name} - {medication.dosage}
            {medication.name === "DOLO" || medication.name === "OXYMED" 
              <div>This is an elder user, please prioritize their medication reminders.</div>
            : null}
            <button onClick={() => addReminder(medication, "10:00 AM")}>
              Add 10:00 AM Reminder
            </button>
            <button onClick={() => addReminder(medication, "6:00 PM")}>
              Add 6:00 PM Reminder
            </button>
            {reminders.filter((r) => r.medication.id === medication.id).map(
              (reminder) => (
                <div key={reminder.id}>
                  {reminder.time}
                  <button onClick={() => removeReminder(reminder)}>
                    Remove
                  </button>
                </div>
              )
            )}
            <button onClick={() => trackAdherence(medication)}>
              Taken
            </button>
          </li>
        ))}
      </ul>
      <button onClick={addMedication}>Add Medication</button>
      <button onClick={removeMedication}>Remove Medication</button>
    </div>
  );
};

export default MedicationReminder;

