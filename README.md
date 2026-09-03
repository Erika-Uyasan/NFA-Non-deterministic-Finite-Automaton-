# NFA-Non-deterministic-Finite-Automaton-
Assignment 3
<img width="2233" height="3421" alt="1788405695770" src="https://github.com/user-attachments/assets/f0bcfe09-351d-4897-906e-dd3b5010002e" />
class EpsilonNFA:
    def __init__(self):
        self.start_state = 'q0'
        self.accept_states = {'q5'}

    def epsilon_closure(self, states):
        """Finds all states reachable via epsilon transitions."""
        closure = set(states)
        stack = list(states)
        
        while stack:
            state = stack.pop()
            if state == 'q2' and 'q3' not in closure:
                closure.add('q3')
                stack.append('q3')
                
        return closure

    def move(self, current_states, symbol):
        """Performs state transitions based on input character."""
        next_states = set()
        
        # Map any character that is not '/' or '*' to 'a'
        c = symbol if symbol in {'/', '*'} else 'a'

        for state in current_states:
            if state == 'q0' and c == '/':
                next_states.add('q1')
            elif state == 'q1' and c == '*':
                next_states.add('q2')
            elif state == 'q2' and c == '*':
                next_states.add('q4')
            elif state == 'q3' and (c == 'a' or c == '/'):
                next_states.add('q2')
            elif state == 'q4':
                if c == '*':
                    next_states.add('q4')
                elif c == 'a':
                    next_states.add('q2')
                elif c == '/':
                    next_states.add('q5')

        return next_states

    def accepts(self, input_string):
        """Processes string and returns True if string is accepted."""
        current_states = self.epsilon_closure({self.start_state})

        for char in input_string:
            current_states = self.move(current_states, char)
            current_states = self.epsilon_closure(current_states)
            if not current_states:
                return False  # Trap state / invalid transition

        return bool(current_states & self.accept_states)


# --- Interactive Terminal Checker ---
def main():
    nfa = EpsilonNFA()
    print("==================================================")
    print("   C-Style Comment Checker (ε-NFA Simulator)     ")
    print("==================================================")
    print("Type any string to test. Type 'exit' to quit.\n")

    while True:
        user_input = input("Enter string: ")
        
        if user_input.lower() == 'exit':
            print("Exiting program.")
            break
        
        if nfa.accepts(user_input):
            print(f"-> Result: ACCEPTED ✅\n")
        else:
            print(f"-> Result: REJECTED ❌\n")

if __name__ == "__main__":
    main()
