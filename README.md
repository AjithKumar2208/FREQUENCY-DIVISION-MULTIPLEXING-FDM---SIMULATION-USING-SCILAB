# FREQUENCY-DIVISION-MULTIPLEXING-FDM---SIMULATION-USING-SCILAB

# AIM

To generate multiple message signals, multiplex them using AM-FDM, and recover each channel using bandpass filtering and coherent demodulation in Scilab.

# APPARATUS REQUIRED

1. Computer with Scilab
2. Scilab script (.sci file)
3. Basic DSP knowledge (filters, convolution, FFT)

# ALGORITHM

1. Generate message signals ( m_i(t) ).
2. Multiply each with its carrier to form AM-DSB.
3. Sum all channels → FDM composite.
4. For each channel:
   a. Band-pass filter around carrier.
   b. Multiply with same carrier (coherent detection).
   c. Low-pass filter to recover baseband.
5. Plot message, FDM, recovered outputs.

# PROCEDURE 

1. Define sampling rate, time vector, message and carrier frequencies.
2. Generate sinusoidal messages and modulate them with cosine carriers.
3. Add all modulated signals to form the composite FDM signal.
4. Design FIR bandpass filter to isolate each channel.
5. Demodulate by mixing with original carrier and low-pass filtering.
6. Plot original messages, FDM signal and recovered signals.
# PROGRM
~~~
clc; clear; close;
fs=80000; N=floor(0.05*fs); t=(0:N-1)/fs;
fm=[450 460 470 480 490 500];
fc=[4500 4600 4700 4800 4900 5000];
Am=[5.8 5.9 6.0 6.1 6.2 6.3];
Ac=[11.6 11.8 12.0 12.2 12.4 12.6];
num=length(fm);
for i=1:num
    m(i,:)=Am(i)*sin(2*%pi*fm(i)*t);
end
fdm=zeros(1,N);
for i=1:num
    fdm = fdm + Ac(i)*cos(2*%pi*fc(i)*t).*m(i,:);
end
function h=FIR(fc1,fc2,fs,M,mode)
    n=-M:M; L=length(n);
    x1=2*fc1*n/fs; x2=2*fc2*n/fs;
    s1=ones(1,L); s2=ones(1,L);
    for k=1:L
        if x1(k)<>0 then s1(k)=sin(%pi*x1(k))/(%pi*x1(k)); end
        if x2(k)<>0 then s2(k)=sin(%pi*x2(k))/(%pi*x2(k)); end
    end
    lp1=(2*fc1/fs)*s1; lp2=(2*fc2/fs)*s2;
    w=(0.54-0.46*cos(2*%pi*(n+M)/(2*M)));
    if mode==1 then h=lp1.*w;
    else h=(lp2-lp1).*w; end
    h=h/sum(h);
endfunction
M=300;
for i=1:num
    bp=FIR(fc(i)-600,fc(i)+600,fs,M,2);
    iso=conv(fdm,bp,"same");
    mix=iso.*cos(2*%pi*fc(i)*t);
    lp=FIR(800,0,fs,M,1);
    demod(i,:)= (2/Ac(i))*conv(mix,lp,"same");
end
scf(1); clf;
for i=1:num subplot(num,1,i); plot(t,m(i,:)); end
scf(2); clf; plot(t,fdm);
scf(3); clf;
for i=1:num subplot(num,1,i); plot(t,demod(i,:)); end

~~~
# OUTPUT
<img width="1599" height="1081" alt="image" src="https://github.com/user-attachments/assets/e224ac21-ccef-4b32-bd03-a0f71d6e5482" />

<img width="1575" height="984" alt="image" src="https://github.com/user-attachments/assets/785b6f6d-e950-40f7-b130-faa086de573f" />

<img width="1549" height="1074" alt="image" src="https://github.com/user-attachments/assets/27694e36-d5a8-4757-9b9f-ee990693f9da" />

# MANUAL CALCULATION

![WhatsApp Image 2025-11-28 at 22 28 33_d448c0bd](https://github.com/user-attachments/assets/ae726901-3ec8-40e5-8f9b-a1763788154d)

![WhatsApp Image 2025-11-28 at 22 36 21_f1d94e11](https://github.com/user-attachments/assets/79f83e12-eed6-4473-a2c6-0fd37e56d296)

![WhatsApp Image 2025-11-28 at 22 37 06_912cb079](https://github.com/user-attachments/assets/0b8af559-a757-4f42-83a5-a107dcfcfeb2)


# RESULT 

All message frequencies (400 - 500Hz) were successfully multiplexed and individually recovered with minimal distortion using bandpass isolation and coherent demodulation.
